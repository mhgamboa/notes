# Mongoose

1. [Connecting Mongoose](https://github.com/mhgamboa/notes/blob/main/Backend/mongoose.md#Connecting_Mongoose)
2. [Mongoose Models](<(https://github.com/mhgamboa/notes/blob/main/Backend/mongoose.md#Mongoose_Models)>)

Run `npm i mongoose` to install

## Connecting Mongoose

1. in `root/db/connect.js` file:

```
const mongoose = require("mongoose");

const connectDB = (uri) => {
  return mongoose.connect(uri);
};

module.exports = connectDB;
```

2. In `server.js`:

```
const connectDB = require("./db/connect");
...
const main = async ()=>{
    try{
    await connectDB(process.env.MONGO_URI);
    } catch(e){}
};
main();
```

## Mongoose Models

1. Create a file in `root/models/` for each model you want
2. In each file:

```
const mongoose = require('mongoose');
const optionalRelatedSchema = reqruie('./optionalRelatedShema)

const schemaName = new mongoose.Schema({
    name: {
        type: String,
        required: [true, "Error Message"]
        minlength: 5,
        trim: true
    },
    createBy: {
        type: mongoose.Types.ObjectId,
        ref: optionalRelatedSchema,
        required: true
    },
    email: {
      type: String,
      required: [true, "please provide a name"],
      unique: true,
      match: [
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
        "Please provide a valid email",
      ],
    },
    {timestamps: true} // Optional
})

// Optional Hook
UserSchema.pre("save", async function () {
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
});

// Optional methods
UserSchema.methods.createJWT = function () {
  return jwt.sign(
    { userId: this._id, name: this.name },
    process.env.JWT_SECRET,
    { expiresIn: process.env.JWT_LIFETIME }
  );
};

UserSchema.methods.comparePassword = async function (candidatePassword) {
  const isMatch = await bcrypt.compare(candidatePassword, this.password);
  return isMatch;
};

module.exports = mongoose.model("collection Name", schemaName);
```

3. Import model into relevant controller(s):

```
const Item = require("../models/Item");

const items = await Item.find({ createdBy: userId });
const item = await Item.create({ createdBy: userId, name });
const item = await Item.findOneAndRemove({
createdBy: userId,
_id: itemId,
});
const item = await Item.findOneAndUpdate({})
Item.findOne({})

```
