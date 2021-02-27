# MERN

### Create a virtual environment:
* Create an empty folder `mkdir userProfile` -->  `cd userProfile`
* In terminal type `py -m venv ENV_NAME`
* Activate the virtual environment `call ENV_NAME\Scripts\activate`

### Backend Setup 
* Create a new project, in terminal run `npm init -y` This will create the package.json 
* Install our dependencies `npm install express mongoose cors`
* Create a new file, in terminal run `touch server.js`  

#### server.js
```javascript
const express = require('express');
const cors = require('cors');
const app = express();
const port = 7000;

app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

require('./server/config/profile.config');
require('./server/routes/profile.routes')(app);

app.listen(port, () => console.log(`Listening on port: ${port}`));   
```
* Create `touch .gitignore` file and add 
```
ENV_NAME/
node_modules/ 

```
* Create a folder `mkdir server` -->  `cd server`
* Create four folders `mkdir config controllers models routes`  

#### controllers/profile.controller.js
```javascript
const Project = require('../models/profile.models');

module.exports.index = (req, res) => {
    res.json({
       message: "Hello World"
    });
}
```

#### routes/profile.routes.js
```javascript
const ProfileController = require('../controllers/profile.controller');

module.exports = (app) => {
    app.get('/api', ProfileController.index);
}
```

#### config/profile.config.js
```javascript
const mongoose = require('mongoose');
mongoose.connect("mongodb://localhost/profile_project_db", {
useNewUrlParser: true,
		useUnifiedTopology: true,
		useFindAndModify: false
	})
	.then(() => console.log('Successfully connected to Project Profile'))
	.catch((err) => console.log('Something went wrong when connecting to the database ', err));
```

#### models/profile.models.js
```javascript
const mongoose = require('mongoose');

const PersonSchema = new mongoose.Schema(
    {
        name: { 
            type: String,
            required: [ true, 'Name is required' ],
            minlength: [ 3, "Name must be at least 3 characters long" ],
        }
    }, 
    { timestamps: true }
);
module.exports = mongoose.model('Person', PersonSchema);
```
### Test by running the server
* In terminal run `npm start`
* Check our server at `http://localhost:7000/api/`

### Frontend React Setup 
* Make sure you are in the project directory then in terminal run `npx create-react-app client`  
* cd client `npm install axios moment @reach/router react-bootstrap bootstrap`






