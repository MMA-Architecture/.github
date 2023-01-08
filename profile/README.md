# MMA Architecture 

MMA is an architectural framework for managing communications that do not communicate directly with each other.

Setup:
implement the Symbol of keys in 'ProjectSettings>Player>OtherSettings>ScriptCompilation>ScriptingDefineSymbols => MMA_INT || MMA_STRING'
with this you can use MMA_INT to create int keys or MMA_STRING to use string codes (MMA_STRING is better for prototyping and MMA_INT for a better performance)  

Quick start guide:
1. Middleware is a static partial class, use it for Subscribe and publish Action, Func, Coroutines, Task
2. Module is a sealed partial class, used to as a federated manager in a Scene to handle a specification
3. Application works as a entity of a Module of the scene, these are GameObject/s, children of the Module GameObject


# Layers
- External Modules => Modules (stored as repos / packages) added as "Submodules" and linked with their assembly
- Middleware => Architecture (added as submodule / package manager) => https://github.com/MMA-Architecture/mma.git
- Core Modules => Modules stored stored in the main repo. added reference of Data and Service Assembly for comunication
- Data => Used to store the information used in this project (because is only useful for this project)
- Service => Static classes to manage functions or implementations of Modules, it uses Data's Assembly reference

Example of workflow:
```cs

//module who suscribe to a key, waiting for response 'OnChangeGold'
namespace Core.MenuHeaderUIModule
{
  public sealed partial class MenuHeaderUIModule : Module
  {
    [SerializeField] private Text text_gold = default;
    protected override void OnSubscription(bool condition)
    {
      // OnChangeGold
      Middleware<float>.Subscribe_Publish(condition, Data.Key.OnChangeGold, OnChangeGold);
    }
    private void OnChangeGold(float gold)
    {
      text_gold.text = gold.ToString();
    }
  }
}

//module to simulate the money obtention
namespace Core.FakeModule
{
  public sealed partial class FakeModule : Module
  {  
    protected override void OnSubscription(bool condition){}

    void Start()
    {
      Service.Common.OnChangeGold(100)
    }
  }
}

namespace Service
{
  public static Common // example of a Service
  {
      public static void OnChangeGold(float gold)
      {
        Middleware<float>.Invoke_Publish(Data.Key.OnChangeGold, gold);
      }
  }
}


namespace Data
{
  public static Key
  {
    public const string OnChangeGold = "OnChangeGold" //Example of using MMA_STRING as key
  }
}
```
