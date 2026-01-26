---

id: mongoose-schema-complete
title: Mongoose Schema – Complete Reference Example
sidebar_position: 1
-------------------

# Mongoose Schema – Complete Reference Example

> Exhaustive schema definition and usage example covering **nearly all schema, path, index, virtual, middleware, and model options**.
> Assumes **senior / hardcore Mongoose usage**.

---

## Complete Schema Definition

```js
import mongoose from 'mongoose';
const { Schema, model } = mongoose;

const UserSchema = new Schema(
  {
    // ===== Primitive Types =====
    name: {
      type: String,
      required: true,                  // boolean | function
      minlength: 2,                    // number
      maxlength: 100,                  // number
      trim: true,                      // boolean
      lowercase: true,                 // boolean
      uppercase: false,                // boolean
      match: /[a-z]/i,                 // RegExp
      index: true                      // boolean | object
    },

    age: {
      type: Number,
      min: 0,
      max: 150,
      default: 18,
      validate: {
        validator: Number.isInteger,   // function
        message: 'age must be integer'
      }
    },

    email: {
      type: String,
      unique: true,                    // boolean
      sparse: true,                    // boolean
      required: true,
      immutable: true                  // boolean
    },

    isActive: {
      type: Boolean,
      default: true
    },

    // ===== Dates =====
    lastLoginAt: {
      type: Date,
      default: Date.now,
      expires: 60 * 60 * 24 * 30        // TTL seconds
    },

    // ===== Mixed / Maps =====
    metadata: {
      type: Schema.Types.Mixed         // any
    },

    settings: {
      type: Map,
      of: String                       // any schema type
    },

    // ===== Arrays =====
    tags: {
      type: [String],
      validate: v => v.length <= 10    // custom validator
    },

    roles: [{
      type: String,
      enum: ['user', 'admin']          // string[] | object
    }],

    // ===== Subdocuments =====
    profile: {
      bio: String,
      avatar: String
    },

    addresses: [{
      city: String,
      country: String,
      zip: String
    }],

    // ===== ObjectId / Refs =====
    organizationId: {
      type: Schema.Types.ObjectId,
      ref: 'Organization',             // model name
      index: true
    },

    // ===== Versioned / Soft delete =====
    deletedAt: {
      type: Date,
      select: false
    }
  },
  {
    // ===== Schema Options =====
    timestamps: {
      createdAt: 'created_at',         // boolean | string
      updatedAt: 'updated_at'
    },
    versionKey: '__v',                 // string | false
    optimisticConcurrency: true,       // boolean
    strict: 'throw',                   // true | false | 'throw'
    strictQuery: true,                 // boolean
    minimize: true,                    // boolean
    autoIndex: false,                  // boolean (prod)
    autoCreate: false,                 // boolean
    bufferCommands: true,              // boolean
    bufferTimeoutMS: 10000,            // number
    capped: false,                     // false | { size, max }
    collection: 'users',               // string
    discriminatorKey: '__type',        // string
    shardKey: { organizationId: 1 },   // object
    read: 'primary',                   // string
    writeConcern: { w: 'majority' },   // object

    toJSON: {
      virtuals: true,
      getters: false,
      transform(doc, ret) {
        delete ret._id;
        return ret;
      }
    },

    toObject: {
      virtuals: true
    }
  }
);
```

---

## Index Definitions

```js
// single
UserSchema.index({ email: 1 }, { unique: true });

// compound
UserSchema.index({ organizationId: 1, email: 1 });

// partial
UserSchema.index(
  { deletedAt: 1 },
  { partialFilterExpression: { deletedAt: { $exists: true } } }
);
```

---

## Virtuals

```js
UserSchema.virtual('isDeleted').get(function () {
  return !!this.deletedAt;
});
```

---

## Middleware (Hooks)

```js
// document
UserSchema.pre('save', function (next) {
  this.updated_at = new Date();
  next();
});

// query
UserSchema.pre(/^find/, function () {
  this.where({ deletedAt: null });
});

// error
UserSchema.post('save', function (err, doc, next) {
  if (err) return next(err);
  next();
});
```

---

## Statics

```js
UserSchema.statics.findActive = function () {
  return this.find({ isActive: true });
};
```

---

## Methods

```js
UserSchema.methods.softDelete = function () {
  this.deletedAt = new Date();
  return this.save();
};
```

---

## Model Creation

```js
export const User = model('User', UserSchema);
```

---

## Usage Examples

```js
// create
const user = await User.create({
  name: 'john',
  email: 'john@test.com',
  roles: ['user'],
  organizationId: orgId
});

// update
await User.updateOne(
  { _id: user._id },
  { $set: { isActive: false } },
  { runValidators: true }
);

// find + lean
const users = await User.find()
  .select('name email')
  .lean();

// transaction usage
const session = await mongoose.startSession();
await session.withTransaction(async () => {
  await user.softDelete({ session });
});

session.endSession();
```

---

## Projection (Field Selection) Examples

```js
// include fields (string)
await User.find({}, 'name email');

// include fields (object)
await User.find({}, { name: 1, email: 1 });

// exclude fields
await User.find({}, { password: 0, deletedAt: 0 });

// mix is NOT allowed (_id exception)
await User.find({}, { name: 1, _id: 0 });

// select chaining
await User.find()
  .select('name email')
  .select('-deletedAt');

// projection with lean
await User.find({}, { name: 1 })
  .lean({ getters: false, virtuals: false });

// projection in populate
await User.find()
  .populate({
    path: 'organizationId',
    select: 'name status'   // projection on populated doc
  });

// aggregation $project
await User.aggregate([
  { $match: { isActive: true } },
  {
    $project: {
      name: 1,
      email: 1,
      isDeleted: { $cond: [{ $ifNull: ['$deletedAt', false] }, true, false] }
    }
  }
]);
```

---

---

## Notes

* This schema intentionally combines **most real-world options** in one place
* Not all options should be used together in production
* Treat this as a **reference**, not a template

## Caveats

* Overusing schema features increases cognitive load
* Global defaults matter as much as schema options
* Performance must be validated with real workloads

---

## Discriminators (Inheritance)

```js
// base
const BaseUser = model('BaseUser', UserSchema);

// child
const AdminUser = BaseUser.discriminator(
  'AdminUser',
  new Schema({ permissions: [String] })
);

await AdminUser.create({ name: 'admin', email: 'a@test.com', permissions: ['*'] });
```

---

## Populate (Advanced)

```js
// populate with match + options
await User.find()
  .populate({
    path: 'organizationId',
    match: { isActive: true },
    select: 'name',
    options: { lean: true }
  });

// deep populate
await User.find().populate({
  path: 'organizationId',
  populate: { path: 'ownerId', select: 'email' }
});
```

---

## Query Helpers

```js
UserSchema.query.active = function () {
  return this.where({ isActive: true });
};

await User.find().active();
```

---

## Getters & Setters

```js
const PriceSchema = new Schema({
  price: {
    type: Number,
    set: v => Math.round(v),      // setter
    get: v => v / 100             // getter
  }
}, { toJSON: { getters: true } });
```

---

## Validation Nuances

```js
// conditional required
required: function () {
  return this.roles.includes('admin');
}

// async validator
validate: {
  validator: async v => await checkExternal(v)
}
```

---

## Aggregation Helpers

```js
UserSchema.statics.activeAgg = function () {
  return this.aggregate([{ $match: { isActive: true } }]);
};
```

---

## Explain / Index Hints

```js
await User.find({ email: 'a@test.com' })
  .hint({ email: 1 })
  .explain('executionStats');
```
