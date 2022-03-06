[<< Back to menu](SOLID.md)

# Open-Closed Principle
Classes should be opened for extension, but closed for modification. So the classes shouldn't be modified each time you have a new functionality, but yo can extend that class and add the new func to the existing code, so you are not affecting the existing code .

##¿How is it?
if there is an existing class `Electric Engine` and the specialist need to add a new mode to alter the engine from the computer so you are not going to alter the entire engine, you add a new _Feature_:

    public class ElectricEngine {
        public ElectricEngine(){}
        public void Start(){
            //Do some stuff to start the vehicle.
        }
        public void ShutDown(){
            //shuting down the vehicle.
        }
        // You shouldn't alter the existing class to add methods.
        public void AddModeToComputer(){}
    }
# ¿How to Fix it?
    // So on you should extend the class to add the new func:
    public class ElectricEngineWithNewMode : ElectricEngine {
        public void AddModeToComputer() {
            // Do some stuff to add mode to the computer hear.
        }
    }

[<< Back to menu](SOLID.md)