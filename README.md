This project is a backend application that simulates a Trello-like task management system.
It allows multiple users to manage their work using boards, lists, and tasks.

The system helps teams organize workflow efficiently by categorizing tasks into different stages such as To-Do, In-Progress, and Done. Users can create tasks, update them, assign members, and track activity history in real time.


hintro-project
   server.js
   package.json
   .env

config
      db.js

 models
     Task.js

routes
     taskRoutes.js

 controllers
     taskController.js

By using  below command install packages

npm install express mongoose cors dotenv nodemon


Package.json

{
  "name": "hintro-project",
  "version": "1.0.0",
  "description": "Trello Clone Backend",
  "main": "server.js",
  "scripts": {
    "dev": "nodemon server.js",
    "start": "node server.js"
  },
  "author": "Pranusha",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^17.0.0",
    "express": "^4.19.2",
    "mongoose": "^8.0.0",
    "nodemon": "^3.0.0"
  }
}


.env file

PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/hintro


config/db.js

const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB Connected");
  } catch (error) {
    console.log(error);
    process.exit(1);
  }
};

module.exports = connectDB;


models/task.js

const mongoose = require("mongoose");

const taskSchema = new mongoose.Schema({
  title: String,
  description: String,
  status: {
    type: String,
    enum: ["todo", "progress", "done"],
    default: "todo"
  },
  assignedTo: String,
  createdAt: {
    type: Date,
    default: Date.now
  }
});

module.exports = mongoose.model("Task", taskSchema);


controllers/taskcontrollers.java

const Task = require("../models/Task");

// Create Task
exports.createTask = async (req, res) => {
  const task = await Task.create(req.body);
  res.json(task);
};

// Get Tasks
exports.getTasks = async (req, res) => {
  const tasks = await Task.find();
  res.json(tasks);
};

// Update Task
exports.updateTask = async (req, res) => {
  const task = await Task.findByIdAndUpdate(req.params.id, req.body, { new: true });
  res.json(task);
};

// Delete Task
exports.deleteTask = async (req, res) => {
  await Task.findByIdAndDelete(req.params.id);
  res.json({ message: "Task deleted" });
};


routers/taskrouters.js

const express = require("express");
const router = express.Router();
const {
  createTask,
  getTasks,
  updateTask,
  deleteTask
} = require("../controllers/taskController");

router.post("/", createTask);
router.get("/", getTasks);
router.put("/:id", updateTask);
router.delete("/:id", deleteTask);

module.exports = router;


server.js

const express = require("express");
const cors = require("cors");
const dotenv = require("dotenv");
const connectDB = require("./config/db");

dotenv.config();
connectDB();

const app = express();
app.use(cors());
app.use(express.json());

app.get("/", (req, res) => {
  res.send("Hintro Trello Clone Backend Running 🚀");
});

app.use("/api/tasks", require("./routes/taskRoutes"));

const PORT = process.env.PORT || 5000;

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));



## How to Run

1. npm install
2. Create .env file
3. npm run dev
4. Open http://localhost:5000


output:
Hintro Trello Clone Backend Running 
Server running on port 5000
MongoDB Connected


Test in postman

In Postman:

Method → POST

Body → raw → JSON

{
  "title": "Build Login Page",
  "description": "Design UI",
  "status": "todo",
  "assignedTo": "Pranusha"
}
{
  "_id": "65f2c9c123abc...",
  "title": "Build Login Page",
  "description": "Design UI",
  "status": "todo",
  "assignedTo": "Pranusha",
  "createdAt": "2026-02-16T14:30:00.000Z",
  "__v": 0
}

DELETE TASK (DELETE)



Click Send

Response:

{
  "message": "Task deleted"
}


UPDATE TASK (PUT)

Copy _id from GET response

Example:

65f2c9c123abc



Body:
{
  "status": "progress"
}


Click Send

