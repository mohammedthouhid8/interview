import express from "express";
import mongoose from "mongoose";
import router from "./routes/UserRoute";
import blogRouter from "./routes/BlogRoute";
const app = express();
app.use(express.json());
app.use("/api/user", router);
app.use("/api/blog", blogRouter);
mongoose
  .connect(
    "mongodb+srv://admin:1FY9Pi6EUdF3m8u2@cluster0.4fgtegn.mongodb.net/Blog?retryWrites=true&w=majority"
  )
  .then(() => app.listen(8080))
  .then(() =>
    console.log("database connected and listen at localhost port 8080")
  )
  .catch((err) => console.log(err));
