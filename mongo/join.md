# join collections

```
users:
{    
    "_id" : ObjectId("5684f3c454b1fd6926c324fd"),
    "email" : "admin@gmail.com",
    "userId" : "AD",
    "userName" : "admin"
}

userinfo:
{
    "_id" : ObjectId("56d82612b63f1c31cf906003"),
    "userId" : "AD",
    "phone" : "0000000000"
}

userrole:
{
    "_id" : ObjectId("56d82612b63f1c31cf906003"),
    "userId" : "AD",
    "role" : "admin"
}
```

```
db.users.aggregate([

    // Join with user_info table
    {
        $lookup:{
            from: "userinfo",       // other table name
            localField: "userId",   // name of users table field
            foreignField: "userId", // name of userinfo table field
            as: "user_info"         // alias for userinfo table
        }
    },
    {   $unwind:"$user_info" },     // $unwind used for getting data in object or for one record only

    // Join with user_role table
    {
        $lookup:{
            from: "userrole", 
            localField: "userId", 
            foreignField: "userId",
            as: "user_role"
        }
    },
    {   $unwind:"$user_role" },

    // define some conditions here 
    {
        $match:{
            $and:[{"userName" : "admin"}]
        }
    },

    // define which fields are you want to fetch
    {   
        $project:{
            _id : 1,
            email : 1,
            userName : 1,
            userPhone : "$user_info.phone",
            role : "$user_role.role",
        } 
    }
]);
```