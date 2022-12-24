# Business Use Case

The client will enter our website, and enter their personal information like age, car type, car price and so on. After they submit the form, they will get a price for their insurance. The price they get will change based on not only their input information, but also the time--different training dataset as time past.

# Project Structure

`Frontend`: React + Javascript

link：https://github.com/fzfzlfz/Frontend.git

`BackEnd`: Express

link：https://github.com/fzfzlfz/Backend.git

- `app.mjs` -- file which handle API
- `person_model.sav` + `predict_person.py` + `train_person.py` -- Machine Learning
- `price.txt` -- result of the machine learning

![image-20221223010648487](.\dbproject\image-20221223010648487.png)

(Frontend & Backend run in different port)

`Database`: MySQL

- We utilize **ORM** which is prisma in backend. You can see this in the picture up there. 

- to prevent the data leak, we hash and salt the user password to improve the security.

- schema we use

  ```mysql
  CREATE TABLE users (
    id int NOT NULL AUTO_INCREMENT,
    email varchar(50) NOT NULL,
    password varchar(50) NOT NULL,
    photo varchar(50) NOT NULL,
    PRIMARY KEY (id)
  );
  
  CREATE TABLE claims (
    id varchar(50) NOT NULL,
    insuranceId varchar(50) NOT NULL,
    Date varchar(50) NOT NULL,
    Location varchar(50) NOT NULL,
    Amount varchar(50) NOT NULL,
    userid int NOT NULL,
    PRIMARY KEY (id)
  );
  
  CREATE TABLE insurances (
    id varchar(50) NOT NULL,
    firstName varchar(10) NOT NULL,
    lastName varchar(10) NOT NULL,
    gender varchar(10) NOT NULL,
    age int NOT NULL,
    vtype varchar(50) NOT NULL,
    vprice varchar(30) NOT NULL,
    liscenceN varchar(30) NOT NULL,
    district varchar(20) NOT NULL,
    collision boolean NOT NULL,
    userid int NOT NULL,
    PRIMARY KEY (id)
  );
  
  
  ```

  

`Machine Learning`:  name is "person_model.sav"
  Required packages: numpy, sklearn, pandas, sodapy, pickle
  Python version: 3.8.8

 With the dataset Motor Vehicle Collisions – Person from NYC OpenData (src: https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Person/f55k-p6yu), We train a MLPClassifier model to predict the risk of vehicle collision of the person based on sex and age. For sex, we label Female for number 0 and male for number 1. We count the number of collisions of each age-sex group and label them 1-5 based on the number. From 1 to 5, the risk of collision increases.
 The dataset is updated daily, and our system will rerun the training process every week. It will always get the newest 100000 data to train the model.
 We also wanted to train another model to predict the risk base on the type of the vehicle and the borough of New York that the vehicle belongs to. But we met some difficulty when encoding those two features into numerical values.



`end-to-end RA`: 

​	After the frontend send the information user fills in, the backend will route into `getPrice`

  What the `getPrice` does is --> fetch the information from the request, feed the data as args into the .py machine learning file which runs the training. Then, the output will be in the file `price.txt `which will be number 1 to 5, which is mapped into different ratio to the original price for the car type.

  Then, the backend will calculate the ratio and original price. And  then, it will send the price back to the frontend so the users can see the price.

--------------------

# How to run it?

1. unzip "Frontend" and "Backend"
2. for "Frontend", enter in the terminal `npm install` to install all dependencies
3. for "Backend", enter in the terminal `npm install` to install all dependencies
4. for MySQL, remember to change codes in "Backend" in the directory `./prisma/schema.prisma` in line 10 to allow it access to your MySQL
5. for "Backend", enter in the terminal `node app.mjs` to run the backend
6. for "Frontend", enter in the terminal `npm start` to run the frontend. Tip: it may remind you that it will run in different port because 3000 is occupied

# Some Glimpse

### Instructions for web

> Login/Register

![image-20221223005559307](.\dbproject\image-20221223005559307.png)

1. Register First
2. Then you can login

> Fill the form to know price for you

![image-20221223005845671](.\dbproject\image-20221223005845671.png)

> See the price

After you fill this form, you can see the price shown there

For example,

![image-20221223010001530](.\dbproject\image-20221223010001530.png)

> Others

![image-20221223010118256](.\dbproject\image-20221223010118256.png)
