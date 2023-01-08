# MMA Architecture 
### MMA is an architectural framework for managing communications that do not communicate directly with each other.

Setup:
1. Add [MMA](https://github.com/MMA-Architecture/mma.git) with PackageManager or Sourcetree as Submodule.
2. Add others modules (example => [MMA.Scenes](https://github.com/MMA-Architecture/mma.scenes.git)
3. implement the Symbol of keys in 'ProjectSettings>Player>OtherSettings>ScriptCompilation>ScriptingDefineSymbols => MMA_INT || MMA_STRING
(MMA_STRING is better for prototyping and MMA_INT for a better performance, maybe in next releases will exist MMA_BYTE for best performance)

Quick start guide:
1. Middleware is a class for pub/sub Action, Func, Coroutines, Task
2. Module is used as a federated manager in a Scene to handle a specification
3. Application works as a entity of a Module of the scene, these are GameObject/s, children of the Module GameObject

# Example
```cs
namespace Core.MenuHeaderUIModule { //module who suscribe to a key, waiting for response 'OnChangeGold'
  public sealed partial class MenuHeaderUIModule : Module {
    [SerializeField] private Text text_gold = default;
    protected override void OnSubscription(bool condition) => Middleware<float>.Subscribe_Publish(condition, Data.Key.OnChangeGold, OnChangeGold);
    private void OnChangeGold(float gold) => text_gold.text = gold.ToString();
  }
}
namespace Core.FakeModule{ //module to simulate the money obtention
  public sealed partial class FakeModule : Module {  
    protected override void OnSubscription(bool condition){}
    void Start() =>Service.Common.OnChangeGold(100)
  }
}
namespace Service { //Static classes to manage functions or implementations of Modules, it uses Data's Assembly reference
  public static Common {
      public static void OnChangeGold(float gold) => Middleware<float>.Invoke_Publish(Data.Key.OnChangeGold, gold);
  }
}
namespace Data { // Used to store the information used in this project (because is only useful for this project)
  public static Key {
    public const string OnChangeGold = "OnChangeGold" //Example of using MMA_STRING as key
  }
}
```
