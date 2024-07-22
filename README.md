# copia_proyecto1

A new Flutter project.

To test this backend, you can use a tool like Postman or write test scripts using a testing framework like Mocha, Chai, or Jest for a more automated approach. Below are examples of both methods.

Using Postman
Register a User

Method: POST
URL: http://localhost:3000/register
Body (JSON):
json
Copiar código
{
  "username": "testuser",
  "email": "testuser@example.com",
  "password": "password123"
}
Login a User

Method: POST
URL: http://localhost:3000/login
Body (JSON):
json
Copiar código
{
  "email": "testuser@example.com",
  "password": "password123"
}
Response: Should return a token if login is successful.
Add a Book

Method: POST
URL: http://localhost:3000/add-book
Body (JSON):
json
Copiar código
{
  "title": "Test Book",
  "description": "This is a test book description",
  "author": "Test Author",
  "publicationDate": "2024-01-01",
  "link": "http://example.com/testbook"
}
Get User Details

Method: GET
URL: http://localhost:3000/user-details
Headers:
Authorization: Bearer YOUR_JWT_TOKEN (Replace YOUR_JWT_TOKEN with the token received from the login request)
Automated Testing with Mocha and Chai
Install Mocha, Chai, and Chai HTTP

bash
Copiar código
npm install mocha chai chai-http
Create a Test Script

Create a file named test.js with the following content:

javascript
Copiar código
const chai = require('chai');
const chaiHttp = require('chai-http');
const app = require('./app'); // adjust the path as needed
const { expect } = chai;

chai.use(chaiHttp);

let token;

describe('Backend API Tests', () => {
  it('should register a user', (done) => {
    chai.request(app)
      .post('/register')
      .send({
        username: 'testuser',
        email: 'testuser@example.com',
        password: 'password123'
      })
      .end((err, res) => {
        expect(res).to.have.status(201);
        expect(res.body).to.have.property('message').eql('Usuario registrado exitosamente');
        done();
      });
  });

  it('should login a user', (done) => {
    chai.request(app)
      .post('/login')
      .send({
        email: 'testuser@example.com',
        password: 'password123'
      })
      .end((err, res) => {
        expect(res).to.have.status(200);
        expect(res.body).to.have.property('token');
        token = res.body.token;
        done();
      });
  });

  it('should add a book', (done) => {
    chai.request(app)
      .post('/add-book')
      .set('Authorization', `Bearer ${token}`)
      .send({
        title: 'Test Book',
        description: 'This is a test book description',
        author: 'Test Author',
        publicationDate: '2024-01-01',
        link: 'http://example.com/testbook'
      })
      .end((err, res) => {
        expect(res).to.have.status(201);
        expect(res.body).to.have.property('title').eql('Test Book');
        done();
      });
  });

  it('should get user details', (done) => {
    chai.request(app)
      .get('/user-details')
      .set('Authorization', `Bearer ${token}`)
      .end((err, res) => {
        expect(res).to.have.status(200);
        expect(res.body).to.have.property('username').eql('testuser');
        expect(res.body).to.have.property('email').eql('testuser@example.com');
        done();
      });
  });
});
Run the Tests

Add a test script to your package.json:

json
Copiar código
"scripts": {
  "test": "mocha test.js"
}
Then run the tests:

bash
Copiar código
npm test

Navigate to the Correct Directory

Change your directory to backend/mybackend where the package.json file is located:

bash
Copiar código
cd backend/mybackend
Run the Tests

After navigating to the correct directory, run the tests with:

bash
Copiar código
npm test
