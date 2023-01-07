Node server, Mongo & Mongoose
========================================

GitHub package.json version Package.json express version Package.json mongodb version Package.json mongoose version GitHub language count GitHub top language GitHub repo size GitHub code size in bytes Last commit Website MIT License Twitter Follow

Workflow badge PRs Welcome

Created from the FCC repository, to compile the lessons about MongoDB and Mongoose in NodeJS.
ko-fi

Start with an empty repository and making the git init as follows:

git init
git clone https://github.com/Estebmaister/mongoose.git
Adding the files from the original repo in FCC and start to coding.

Scripts
To install all the dependencies:

npm install
To run the server static

npm start
To run the server with dynamic refresh

npm run demon
Challenges
Table of Contents
Install and Set Up Mongoose
Create a Model
Create and Save a Record of a Model
Create Many Records with model.create()
Use model.find() to Search Your Database
Use model.findOne() to Return a Single Matching Document from Your Database
Use model.findById() to Search Your Database By _id
Perform Classic Updates by Running Find, Edit, then Save
Perform New Updates on a Document Using model.findOneAndUpdate()
Delete One Document Using model.findByIdAndRemove
Delete Many Documents with model.remove()
Chain Search Query Helpers to Narrow Search Results
1. Install and Set Up Mongoose
Add mongodb and mongoose to the project’s package.json. Then require mongoose. Store your MongoDB Atlas database URI in the private .env file as MONGO_URI. Surround the the URI with single or double quotes and make sure no space exists between both the variable and the = and the value and =. Connect to the database using the following syntax:

require("dotenv").config();
const mongoose = require("mongoose");

mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
⬆ back to top

2. Create a Model
CRUD Part I - CREATE

First of all we need a Schema. Each schema maps to a MongoDB collection. It defines the shape of the documents within that collection. Schemas are building block for Models. They can be nested to create complex models, but in this case we’ll keep things simple. A model allows you to create instances of your objects, called documents.

Glitch is a real server, and in real servers the interactions with the db happen in handler functions. These function are executed when some event happens (e.g. someone hits an endpoint on your API). We’ll follow the same approach in these exercises. The done() function is a callback that tells us that we can proceed after completing an asynchronous operation such as inserting, searching, updating or deleting. It’s following the Node convention and should be called as done(null, data) on success, or done(err) on error. Warning - When interacting with remote services, errors may occur!

/* Example */

var someFunc = function (done) {
  //... do something (risky) ...
  if (error) return done(error);
  done(null, result);
};
⬆ back to top

3. Create and Save a Record of a Model
In this challenge you will have to create and save a record of a model.

Create a document instance using the Person constructor you built before. Pass to the constructor an object having the fields name, age, and favoriteFoods. Their types must conform to the ones in the Person Schema. Then call the method document.save() on the returned document instance. Pass to it a callback using the Node convention. This is a common pattern, all the following CRUD methods take a callback function like this as the last argument.

/* Example */

// ...
person.save(function (err, data) {
  //   ...do your stuff here...
});
⬆ back to top

4. Create Many Records with model.create()
Sometimes you need to create many instances of your models, e.g. when seeding a database with initial data. Model.create() takes an array of objects like [{name: 'John', ...}, {...}, ...] as the first argument, and saves them all in the db.

Create many people with Model.create(), using the function argument arrayOfPeople.

⬆ back to top

5. Use model.find() to Search Your Database
Find all the people having a given name, using Model.find() -> [Person]

In its simplest usage, Model.find() accepts a query document (a JSON object) as the first argument, then a callback. It returns an array of matches. It supports an extremely wide range of search options. Check it in the docs. Use the function argument personName as search key.

⬆ back to top

6. Use model.findOne() to Return a Single Matching Document from Your Database
Model.findOne() behaves like .find(), but it returns only one document (not an array), even if there are multiple items. It is especially useful when searching by properties that you have declared as unique.

Find just one person which has a certain food in the person's favorites, using Model.findOne() -> Person. Use the function argument food as search key.

⬆ back to top

7. Use model.findById() to Search Your Database By _id
When saving a document, mongodb automatically adds the field _id, and set it to a unique alphanumeric key. Searching by _id is an extremely frequent operation, so mongoose provides a dedicated method for it.

Find the (only!!) person having a given _id, using Model.findById() -> Person. Use the function argument personId as the search key.

⬆ back to top

8. Perform Classic Updates by Running Find, Edit, then Save
In the good old days this was what you needed to do if you wanted to edit a document and be able to use it somehow e.g. sending it back in a server response. Mongoose has a dedicated updating method : Model.update(). It is bound to the low-level mongo driver. It can bulk edit many documents matching certain criteria, but it doesn’t send back the updated document, only a ‘status’ message. Furthermore it makes model validations difficult, because it just directly calls the mongo driver.

Find a person by \_id ( use any of the above methods ) with the parameter personId as search key. Add "hamburger" to the list of the person's favoriteFoods (you can use Array.push()). Then - inside the find callback - save() the updated Person.

Note: This may be tricky, if in your Schema, you declared favoriteFoods as an Array, without specifying the type (i.e. [String]). In that case, favoriteFoods defaults to Mixed type, and you have to manually mark it as edited using document.markModified('edited-field'). See Mongoose documentation

⬆ back to top

9. Perform New Updates on a Document Using model.findOneAndUpdate()
Recent versions of mongoose have methods to simplify documents updating. Some more advanced features (i.e. pre/post hooks, validation) behave differently with this approach, so the Classic method is still useful in many situations. findByIdAndUpdate() can be used when searching by Id.

Find a person by Name and set the person's age to 20. Use the function parameter personName as search key.

Note: You should return the updated document. To do that you need to pass the options document { new: true } as the 3rd argument to findOneAndUpdate(). By default these methods return the unmodified object.

⬆ back to top

10. Delete One Document Using model.findByIdAndRemove
Delete one person by the person's _id. You should use one of the methods findByIdAndRemove() or findOneAndRemove(). They are like the previous update methods. They pass the removed document to the db. As usual, use the function argument personId as the search key.

⬆ back to top

11. Delete Many Documents with model.remove()
Model.remove() is useful to delete all the documents matching given criteria.

Delete all the people whose name is “Mary”, using Model.remove(). Pass it to a query document with the name field set, and of course a callback.

Note: The Model.remove() doesn’t return the deleted document, but a JSON object containing the outcome of the operation, and the number of items affected. Don’t forget to pass it to the done() callback, since we use it in tests.

⬆ back to top

12. Chain Search Query Helpers to Narrow Search Results
If you don’t pass the callback as the last argument to Model.find() (or to the other search methods), the query is not executed. You can store the query in a variable for later use. This kind of object enables you to build up a query using chaining syntax. The actual db search is executed when you finally chain the method .exec(). You always need to pass your callback to this last method. There are many query helpers, here we’ll use the most ‘famous’ ones.

Find people who like burrito. Sort them by name, limit the results to two documents, and hide their age. Chain .find(), .sort(), .limit(), .select(), and then .exec(). Pass the done(err, data) callback to exec().

Well Done !! You completed these challenges, let's go celebrate !

⬆ back to top

Further Readings
If you are eager to learn and want to go deeper, You may look at :

Indexes ( very important for query efficiency ),
Pre/Post hooks,
Validation,
Schema Virtuals and Model, Static, and Instance methods,
and much more in the mongoose docs
License
MIT
