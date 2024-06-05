practice-tasks-2-solutions
Problem Statements
1. Retrieve the count of individuals who are active (isActive: true) for each gender.
db.collection.aggregate([
  { $match: { isActive: true } },
  {
    $group: {
      _id: "$gender",
      count: { $sum: 1 },
    },
  },
]);
2. Retrieve the names and email addresses of individuals who are active (isActive: true) and have a favorite fruit of "banana".
db.collection.aggregate([
  { $match: { isActive: true, favoriteFruit: "banana" } },
]);
3. Find the average age of individuals for each favorite fruit, then sort the results in descending order of average age.
db.collection.aggregate([
  {
    $group: {
      _id: "$favoriteFruit",
      averageAge: { $avg: "$age" },
    },
  },
  {
    $sort: { averageAge: -1 },
  },
]);
4. Retrieve a list of unique friend names for individuals who have at least one friend, and include only the friends with names starting with the letter "W".
db.collection.aggregate([
  { $unwind: "$friends" },
  {
    $match: {
      "friends.name": /^W/,
    },
  },
  {
    $group: {
      _id: "$_id",
      uniqueFriends: { $addToSet: "$friends.name" },
    },
  },
]);
5. Use $facet to separate individuals into two facets based on their age: those below 30 and those above 30. Then, within each facet, bucket the individuals into age ranges and sort them by age within each bucket.
db.collection.aggregate([
  {
    $facet: {
      below30: [
        { $match: { age: { $lt: 30 } } },
        {
          $bucket: {
            groupBy: "$age",
            boundaries: [20, 25, 30],
            default: "Other",
            output: {
              names: { $push: "$name" },
            },
          },
        },
       {
         $sort: {age: 1}
       }
       
      ],
      above30: [
        { $match: { age: { $gte: 30 } } },
        {
          $bucket: {
            groupBy: "$age",
            boundaries: [30, 35, 40],
            default: "Other",
            output: {
              names: { $push: "$name" },
            },
          },
        },
      ],
    },
  },
 {
   $sort: {age: 1}
 }
]);
6.Calculate the total balance of individuals for each company and display the company name along with the total balance. Limit the result to show only the top two companies with the highest total balance.
db.collection.aggregate([
  {
    $group: {
      _id: "$company",
      totalBalance: { $sum: { $toDouble: { $substr: ["$balance", 1, -1] } } },
    },
  },
  {
    $sort: { totalBalance: -1 },
  },
  {
    $limit: 2,
  },
]);
About
No description, website, or topics provided.
Resources
 Readme
 Activity
 Custom properties
Stars
 2 stars
Watchers
 1 watching
Forks
 0 forks
Report repository
Releases
No releases published
Packages
No packages published
Contributors
2
@abedinforhan
abedinforhan Mezbaul Abedin Forhan
@saminravi11
saminravi11 Samin Israr Ravi
Footer
