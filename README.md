# CurdApp
work curd operation on book object 
CODE STEPS :
1- download packages 
- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.Design
- Microsoft.EntityFrameworkCore.SqlServer



2- create a class in model folder AppDbContext.cs

  3- connect App with sqlserver
  - Rwote connection string on file appsetting.json
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=IT-PC;Initial Catalog=MyDataBase;Integrated Security=True;Pooling=False;Encrypt=False;Trust Server Certificate=True
}

  -  import entityframework on program.cs
    using Microsoft.EntityFrameworkCore;

  builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));


    4- create a database using Migrations
    dotnet ef migrations add afnan2
    dotnet ef database update


    5- add a table in database 
    - create class book on model folder
    - in class appcontext i wrote the following code
    -  public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
 {
 } // constactor
    -  public DbSet<Book> books { get; set; } 
    
  
- Migration command
- dotnet ef migration add createbook
-  dotnet ef database update

  6- add data using code in AppDbContext add function  OnModelCreating 
     - protected override void OnModelCreating(ModelBuilder modelBuilder)
  {
      modelBuilder.Entity<Book>().HasData(new Book { Id = 1, Title = "live", Description = "focus on your self", Author = "afnan", PRICE = 300 });

      modelBuilder.Entity<Book>().HasData(new Book { Id = 2, Title = "morning",Description="000", Author = "afnan", PRICE = 100 });
      modelBuilder.Entity<Book>().HasData(new Book { Id = 3, Title = "money",Description="nothing", Author = "afnan", PRICE = 10 });

     
  } // method to create a book
- Migration command : dotnet ef migrations add dataseeding
-  dotnet ef database update




Curd Operations




1- Controller
a-in controller wrote a varible: private readonly AppDbContext _db;
-create a constractur :   
public HomeController(AppDbContext db)
    {
        _db = db;
    }

1- wrote index method to display a list of book
   public IActionResult Index()
 {
     var books = _db.books.ToList();
     ViewData["books"] = books;
     return View();
 }

2- method create a book
a- Git: /book/ Create
-    public IActionResult Create()
   {

       return View();

   }
   
b- Post: /book/Create

        [HttpPost]

        public IActionResult Create([Bind("Title", "Description", "Author", "PRICE")] Book b)
        {
            //_db.books.Add(b);
            _db.books.Add(b);
            _db.SaveChanges();
            return RedirectToAction("Index");

        }






3-Edit a book 
a- GIT: /Book/Edit

   public IActionResult Edit(int? id)
   {
       var books = _db.books.ToList().Find(book => book.Id == id);

       if (id == null || books == null)
       {
           ViewData["id"] = id;
           return View("nodata", id);
       }
       {
           ViewData["books"] = books;
           return View(books);

       }

   }
b-  // POST: /Book/edit

        [HttpPost]

        public IActionResult Edit(int id, [Bind("Title", "Description", "Author", "PRICE")] Book b)
        {
            var bookToUpdate = _db.books.Find(id);

            if (bookToUpdate == null || id == null)
            {



                ViewData["id"] = id;
                return View("nodata", id);

            }
            else
            {
                bookToUpdate.Title = b.Title;
                bookToUpdate.Description = b.Description;
                bookToUpdate.Author = b.Author;
                bookToUpdate.PRICE = b.PRICE;
                _db.SaveChanges();
                return RedirectToAction("Index");
            }

        }

4- delete the book
  a- // POST : / BOOKS/DELETE/ID
        [HttpPost]
        public IActionResult Delete(int id)
        {
            var book = _db.books.ToList().FirstOrDefault(p => p.Id == id);
            _db.books.Remove(book);
            _db.SaveChanges();
            return RedirectToAction("Index");
        }



2- Views


create a view for each method :
= index.cshtml
= Create.cshtml
= Edit.cshtml
= nodata.cshtml














  
