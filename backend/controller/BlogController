import mongoose from "mongoose";
import Blog from "../model/Blog";
import User from "../model/User";

export const getAllBlogs = async (req, res, next) => {
  let blog;
  try {
    blog = await Blog.find();
  } catch (error) {
    return console.log(error);
  }
  if (!blog) {
    return res.status(404).json({ message: "There is not blog" });
  }
  return res.status(200).json({ blog });
};

export const addBlog = async (req, res, next) => {
  const { title, description, image, user } = req.body;
  let existingUser;
  try {
    existingUser = await User.findById(user);
  } catch (error) {
    return console.log(error);
  }
  if (!existingUser) {
    return res
      .status(400)
      .json({ message: "Unable to find the User with this ID" });
  }
  const newBlog = new Blog({
    title,
    description,
    image,
    user,
  });
  try {
    // await newBlog.save();
    const session = await mongoose.startSession();
    session.startTransaction();
    await newBlog.save({ session });
    existingUser.blogs.push(newBlog);
    await existingUser.save({ session });
    await session.commitTransaction();
  } catch (error) {
    console.log(error);
    return res.status(500).json({ message: error });
  }
  return res
    .status(201)
    .json({ message: "New Blog Created Successfully", newBlog });
};

export const updateBlog = async (req, res, next) => {
  const { title, description } = req.body;
  const blogId = req.params.id;
  let blog;
  try {
    blog = await Blog.findByIdAndUpdate(blogId, {
      title,
      description,
    });
  } catch (error) {
    return console.log(error);
  }
  if (!blog) {
    return res.status(500).json({ message: "Unable to update" });
  }
  return res.status(200).json({ blog, message: "blog updated successfully" });
};

export const getById = async (req, res, next) => {
  const id = req.params.id;
  let blog;
  try {
    blog = await Blog.findById(id);
  } catch (error) {
    return console.log(error);
  }
  if (!blog) {
    return res.status(404).json({ message: "No Blog Found!!" });
  }
  return res.status(200).json({ blog });
};

export const deleteBlog = async (req, res, next) => {
  const id = req.params.id;
  let deleteBlog;
  try {
    deleteBlog = await Blog.findByIdAndRemove(id).populate("user");
    await deleteBlog.user.blogs.pull(deleteBlog);
    await deleteBlog.user.save();
  } catch (error) {
    return console.log(error);
  }
  if (!deleteBlog) {
    return res.status(500).json({ message: "Unable to delete" });
  }
  return res.status(200).json({ message: " successfully deleted" });
};

export const getByUserId = async (req, res, next) => {
  const userId = req.params.id;
  let userBlogs;
  try {
    userBlogs = await User.findById(userId).populate("blogs");
  } catch (error) {
    return console.log(error);
  }
  if (!userBlogs) {
    return res.status(404).json({ message: " No Blog Found" });
  }
  return res.status(200).json({ blogs: userBlogs });
};
