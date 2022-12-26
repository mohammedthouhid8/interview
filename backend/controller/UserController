import User from "../model/User";
import bcryptjs from "bcryptjs";

export const getAllUser = async (req, res, next) => {
  let users;
  try {
    users = await User.find();
  } catch (error) {
    return console.log(error);
  }
  if (!users) {
    return res
      .status(404)
      .json({ success: false, message: "user can not be found" });
  }
  return res.status(200).json({ users });
};

export const signup = async (req, res, next) => {
  const { name, email, password } = req.body;
  let existingUser;
  // check if the user exists
  try {
    existingUser = await User.findOne({ email });
  } catch (error) {
    return console.log(error);
  }
  if (existingUser) {
    return res.status(400).json({
      success: false,
      message: "User has an account already!! Login ",
    });
  }
  // post a new user
  const hashedPassword = bcryptjs.hashSync(password);
  const user = new User({
    name,
    email,
    password: hashedPassword,
    blogs: [],
  });

  try {
    await user.save();
  } catch (error) {
    return console.log(error);
  }
  return res.status(201).json({ message: "Account succesfully created", user });
};

// controller function for login

export const login = async (req, res, next) => {
  const { email, password } = req.body;
  let existingUser;
  try {
    existingUser = await User.findOne({ email });
  } catch (error) {
    return console.log(error);
  }
  if (!existingUser) {
    return res.status(404).json({
      success: false,
      message: "Incorrect credentials, please try again",
    });
  }
  const isPassword = bcryptjs.compareSync(password, existingUser.password);
  if (!isPassword) {
    return res.status(400).json({
      success: true,
      message: "Incorrect Credentials, Please try again",
    });
  }
  return res.status(200).json({ success: false, message: "Login Successful" });
};
