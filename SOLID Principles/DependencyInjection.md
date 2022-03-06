#Dependency Inversion Principle

Avoid dependency of high level logic from low level logic.

#¿how is it?
You shouldn't have to change a class to access his private properties from the client or the consumer code, if you have to, you're violating the Dependency Inversion Principle.
- **Problem Code:** in the example bellow Research class (High level), implement directly the RelationShips class, but there is only a way 
to access to the relations list, the RelationShips class has to expose their private properties to be used, but it should not be like that.
        

    namespace DependencyInversionPrinciple
    {
        // hl modules should not depend on low-level; both should depend on abstractions
        // abstractions should not depend on details; details should depend on abstractions
        
        public enum Relationship
        {
            Parent,
            Child,
            Sibling
        }
    
        public class Person
        {
            public string Name;
            // public DateTime DateOfBirth;
        }

        public class Relationships // low-level
        {
            private List<(Person,Relationship,Person)> relations
                = new List<(Person, Relationship, Person)>();
        
            public void AddParentAndChild(Person parent, Person child)
            {
              relations.Add((parent, Relationship.Parent, child));
              relations.Add((child, Relationship.Child, parent));
            }
            // shouldn't expose directly his properties, it creates a directly 
            // dependency on the implementation code (high level).
            public List<(Person, Relationship, Person)> Relations => relations;
        }
        
        public class Research
        {
            public Research(Relationships relationships)
            {
                // Shouldn't have to make low level logic at the client or the
                // high level logic. if another type of filter needed you have to 
                // make it from each implementation.
                // high-level: find all of john's children
                var relations = relationships.Relations;
                foreach (var r in relations
                  .Where(x => x.Item1.Name == "John"
                              && x.Item2 == Relationship.Parent))
                {
                    WriteLine($"John has a child called {r.Item3.Name}");
                }
            }
            
            public Research(IRelationshipBrowser browser) {
              foreach (var p in browser.FindAllChildrenOf("John"))
              {
                WriteLine($"John has a child called {p.Name}");
              }
            }
        
            static void Main(string[] args)
            {
              var parent = new Person {Name = "John"};
              var child1 = new Person {Name = "Chris"};
              var child2 = new Person {Name = "Matt"};
        
              // low-level module
              var relationships = new Relationships();
              relationships.AddParentAndChild(parent, child1);
              relationships.AddParentAndChild(parent, child2);
        
              new Research(relationships);
              
            }
        }
    }


#¿How to fix it?
You can abstract the class that represents stores or some kind of data access to an interface that expose methods and manipulate their properties depending on the implementation.

- **Code Solution:**


    namespace DotNetDesignPatternDemos.SOLID.DependencyInversionPrinciple
    {
        // hl modules should not depend on low-level; both should depend on abstractions
        // abstractions should not depend on details; details should depend on abstractions
        
        public enum Relationship
        {
            Parent,
            Child,
            Sibling
        }
    
        public class Person
        {
            public string Name;
            // public DateTime DateOfBirth;
        }
        // Create an interface that allow to share the private properties of the class without exposing them
        // so you garantee that the client code does not have any dependency on low-level code
        public interface IRelationShipBrowser
        {
            IEnumerable<Person> FindAllChildrenOf(string name);

        }

        public class Relationships: IRelationShipBrowser // low-level
        {
            private List<(Person,Relationship,Person)> relations
                = new List<(Person, Relationship, Person)>();
        
            public void AddParentAndChild(Person parent, Person child)
            {
              relations.Add((parent, Relationship.Parent, child));
              relations.Add((child, Relationship.Child, parent));
            }
            // Implement the interface so you can manipulate the code inside the concrete store ore class.
            public List<Person> FindAllChildrenOf(string name){
                return relations
                  .Where(x => x.Item1.Name == name
                              && x.Item2 == Relationship.Parent).select(r => r.item3);
            }
        }
        
        public class Research
        {
            // The constructor does not implement the store directly, instead, it implement the interface and let the 
            // responsability of the private properties to his owner.
            public Research(IRelationShipBrowser browser)
            {
                foreach(var p in browser.FindAllChildrenOf("Jhon")){
                    Console.WriteLine($"Jhon has a child called {p.Name}");
                }
            }
            
            public Research(IRelationshipBrowser browser) {
              foreach (var p in browser.FindAllChildrenOf("John"))
              {
                WriteLine($"John has a child called {p.Name}");
              }
            }
        
            static void Main(string[] args)
            {
              var parent = new Person {Name = "John"};
              var child1 = new Person {Name = "Chris"};
              var child2 = new Person {Name = "Matt"};
        
              // low-level module
              IRelationShipsBrowser relationships = new Relationships();
              relationships.AddParentAndChild(parent, child1);
              relationships.AddParentAndChild(parent, child2);
        
              new Research(relationships);
              
            }
        }
    }
