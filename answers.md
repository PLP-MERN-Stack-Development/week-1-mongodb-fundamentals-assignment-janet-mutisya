## finding books in specific genre
db.books.find({in_stock: false})

## finding books in_stock and published after 2010
 db.books.find({in_stock:true, published_year: {$gt: 2010}})

## finding books published after the year 2010
db.books.find({published_year: {$gt:2010}})

## finding books published by specific authors
db.books.find({
  author: { $in: ["George Orwell", "J.K. Rowling", "Chinua Achebe"] }
})

# updating price of a specific book
db.books.updateOne({ title: "1984" }, { $set: { pages: 430 } })

# updating many 
db.books.updateMany({ published_year: 1925 }, { $set: { published_year: 2010 } })
db.books.updateMany({ published_year: 1949 }, { $set: { published_year: 2030 } })

## deleting one
db.books.deleteOne({published_year:2010})

# query to find books in_stock and published after year 2010
db.books.find({published_year: {$gt:2010}})


## Use projection to return only the title, author, and price fields in your queries
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 })

## Implement sorting to display books by price both ascending and descending
db.books.find(price).sort(-1) 
db.books.find(price).sort(1)

#  Implement a pipeline that groups books by publication decade and counts them
db.books.aggregate([
  {
     $project: {
      decade: { $multiply: [ { $floor: { $divide: ["$published_year", 10] } }, 10 ] }
    }
  },
  {
    $group: {
      _id: "$decade",
      count: { $sum: 1 }
    }
  },
  {
  g
    $sort: { _id: 1 }
  }
])
# Group books by publication decade and count them
db.books.aggregate([
    {
        $group: {
            _id: {
                $floor: { $divide: ["$published_year", 10] }
            },
            count: { $sum: 1 }
        }
    },
    {
        $sort: { _id: 1 }
    }
]);

# Indexing

# Create index on title field
db.books.createIndex({ title: 1 });

# Create compound index on author and published_year
db.books.createIndex({ author: 1, published_year: 1 });

# Demonstrate performance improvement with explain()
# Without index
db.books.find({ title: "To Kill a Mockingbird" }).explain("executionStats");

# With index
db.books.find({ title: "To Kill a Mockingbird" }).explain("executionStats");