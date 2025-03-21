## How to Check if `_id` is ObjectId or String?

Run this in MongoDB shell:

```javascript
db.collectionName.find({}, { _id: 1 }).limit(5);
```

- If `_id` looks like`{ "_id": ObjectId("66a7e7b58ef3ee1506dd07bd") }`, it's an ObjectId.
- If `_id` looks like `{ "_id": "66a7e7b58ef3ee1506dd07bd" }`, it's stored as a string.

## Issue

If imported external data into your MongoDB database and `_id` is stored as a **string** instead of an **ObjectId**, you have two options:

## ✅ A : Convert `_id` Fields to ObjectId (Recommended)

If your` _id` should be an `ObjectId`, convert it back using an **update operation** in MongoDB.

**Convert All String `_ids` to ObjectId in MongoDB Shell**

```javascript
db.collectionName.find().forEach((doc) => {
  db.collectionName.updateOne(
    { _id: doc._id },
    { $set: { _id: ObjectId(doc._id) } }
  );
});
```

**✅ This will change all `_id` values to ObjectId format.**

## ✅ B: MongoDB Compass / Shell

### 🔹 Option 1: Run JavaScript in Compass' Aggregation Pipeline (Manual)

You can** manually** update each document inside **MongoDB Compass**.

**Steps:**

**1. Open MongoDB Compass** and connect to your database.
**2. Select your database →** Click on the collection `(e.g., yourCollection)`.
**3. Click on the "Aggregation" Tab.**
**4. Run this Aggregation Pipeline:**

```json
Copy code
[
    {
        "$set": {
            "_id": {
                "$toObjectId": "$_id"
            }
        }
    }
]
```

5. Click **Run** → This will convert all `_ids` from string to `ObjectId.`

**✅ Now, all `_ids` are converted in MongoDB Compass!**

### 🔹 Option 2: Run JavaScript in Compass' Query Console (Automatic)

MongoDB Compass has a built-in query console where you can **run JavaScript commands**.

**Steps:**

**1. Open MongoDB Compass** and go to your collection.
**2. Click on "Command Line Interface (CLI)".**
**3. Run the following JavaScript command:**

```javascript
db.yourCollection.find({}).forEach((doc) => {
  if (typeof doc._id === "string") {
    let newId = ObjectId(doc._id);
    let newDoc = { ...doc, _id: newId };

    db.yourCollection.insertOne(newDoc); // Insert with ObjectId
    db.yourCollection.deleteOne({ _id: doc._id }); // Delete old

    print("Converted:", doc._id, "→", newId);
  }
});
```

**4. Press Enter** to execute.

**✅ Now, your `_ids` are converted inside Compass!**

### 🔹 Option 3: Export, Edit, and Re-import (Last Resort)

If the above methods don’t work, you can:

**1. Export the collection as JSON.**
**2. Modify `_id` values** in a text editor:

- Replace \_id values with:

```json
{ "id": { "$oid": "66a7e7b58ef3ee1506dd07bd" } }
```

**3. Re-import the data** in MongoDB Compass.

**✅ Now, MongoDB Compass will recognize `_id` as ObjectId. 🚀**

## 🎯 Best Method?

- **For few documents →** Use **Aggregation Pipeline** (Option 1).
- **For many documents →** Use **Query Console** (Option 2).
- **For complex changes →** Use **Export & Re-import** (Option 3).

## 📌 Solution: Convert `_id` String to ObjectId in MongoDB Compass

Go to the **Aggregation Tab in MongoDB Compass**. Follow these steps:

#### ✅ Step 1: Use Aggregation to Convert `_id`

1. Click **"Add Stage"**.
2. Choose **"$set"** stage.
3. In the input box, enter:

```json
{
  "_id": { "$toObjectId": "$_id" }
}
```

4. Click **"Run"** to preview changes.

#### ✅ Step 2: Apply Changes

- If the preview looks correct, **save the aggregation** and apply it to update your documents.

Now that you've successfully converted `_id` from a **string** to **ObjectId** in the aggregation stage, the next step is to **apply these changes permanently** in your MongoDB collection.

**🚀 Steps to Save Changes in MongoDB Compass**

1. Click on **"Export to Language"** (found at the top near "Run").
2. Choose **"Python (PyMongo)" or "JavaScript (Node.js)"** depending on your backend.
3. Copy the generated code.
4. **Run the script** in your backend or MongoDB shell to update your collection.

**If trying to convert the `_id` field from **string** to ObjectId using the `$toObjectId` operator in MongoDB Shell (Mongosh). However, this aggregation doesn't actually modify your collection—it only transforms the output temporarily.**

#### ✅ How to Permanently Convert \_id to ObjectId

Since MongoDB doesn't allow direct updates to `_id`, you need to **re-insert the documents with the correct type**.

**1️⃣ Create a New Collection with ObjectId \_id**
Run the following in **Mongosh**:

```javascript
<!-- db.products.aggregate([ -->
db["products"].aggregate([
    {
        $addFields: {
            _id: { $toObjectId: "$_id" }
        }
    },
    {
        $out: "products_new"  // Creates a new collection with converted _id
    }
]);
```

This creates a new collection products_new where \_id is an **ObjectId**.

**2️⃣ Drop the Old Collection (⚠️ Backup First!)**
Once you verify that products_new has the correct \_id format, **replace the original collection**:

```javascript
db.products.drop(); // Deletes old collection
db.products_new.renameCollection("products"); // Rename new collection
```

**3️⃣ Verify**
Run:

```javascript
db.products.findOne();
```

**✅ \_id should now be of type ObjectId. 🚀**
