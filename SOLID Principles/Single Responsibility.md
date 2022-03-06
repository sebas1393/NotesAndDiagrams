# Single Responsibility Principle

- A class should only have one reason to change.
- Separation of concerns; different classes handling different, independent task/problems.

So a class called `Document` has the responsibility to open, close, read and write on a document.
But it requires to be printed so the bad approach it's to create a new method on the same class to get the printed document, so the `Document` class it's now a document printer and a document. e.g.

    public class Document {
        private string _name;
        private string _body;
        public Document(string name, string body){
            _name = name;
            _body = body;
        }
        public string read(){
            Console.WriteLine($"Title {_name}");
            Console.WriteLine(body);
        }
        public void write(string text){
            _body += body;
        }
        // So on the document shouldn't take care of the work of a printer so you should create a new class 
        // to take care of that task and you can make your live easier.
        public string print(){
            // Doing some stuff to print.
        }
    }
    
instead of the method inside document class you can create a new class Printer so you can take care of print documents on that class, e.g.:

    public class Printer {
        public Printer() {   
            
        }
        public void print(Document document){
            // You should put some stuff to print here.
        }
    }