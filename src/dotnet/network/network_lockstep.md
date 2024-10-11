https://github.com/SnpM/LockstepFramework




# class GameManager
              void Awake()
              {
                     NetworkHelper networkHelper = gameObject.GetComponent<NetworkHelper>();
                     if (networkHelper == null)
                           networkHelper = gameObject.AddComponent<DefaultNetworkHelper>();
                     //Currently deterministic but not guaranteed by Unity
                     // may be add as serialized Array as property?  [SerializeField] private BehaviourHelper[] helpers; ?
                     BehaviourHelper[] helpers = this.gameObject.GetComponentsInChildren<BehaviourHelper>();
                     LockstepManager.Initialize(helpers, networkHelper);
              }


# class LockstepManager
*******
void Simulate()
{

//    InfluenceCount ???????
//        FrameManager.Simulate
// ==>
//    List<Command> commands = FrameManager.Simulate(InfluenceCount);
//    commands.ForEach(LockStepManager.Exdecute);


    FrameCount == 0
        GameStart() => BehaviourHelperManager.GameStart ();

    BehaviourHelperManager.Simulate ();
    AgentController.Simulate ();
    PhysicsManager.Simulate ();
    CoroutineManager.Simulate ();
    InfluenceManager.Simulate ();
    ProjectileManager.Simulate ();

    LateSimulate ();

    FrameCount++;
}

# class Command
public byte ControllerID;
public ushort InputCode;

* BiDictionary <->

* TData : ICommandData
* private static BiDictionary<Type, ushort> RegisteredData = new BiDictionary<Type, ushort>(); // TData -> data_id
* private Dictionary<ushort,FastList<ICommandData>> ContainedData = new Dictionary<ushort,FastList<ICommandData>>(); // data_id -> [ICommandData]




# class FrameManager
* frame storage - resizable
* Simulate
    * frame.Commands { command =>  LockStepManager.Execute(command) }
* TweakFramerate () - modify Time.timescale - depends on ForeSight


# class CommandManager
Packet : [frame_count, [Frame]]
* recieve packet
* send/flush(send packet) - ClientManager.Distribute(bufferdBytes)


# class ClientManager
* SimulateNetworking
* NetworkHelper - send / OnFrameData / OnInitData


# etc
Packet : [frame_count, [Frame]]
Frame : [Command]
Command : [ControllerID, InputCode, ContainedTypesCount, [[i_command_data_id, command_data_count, [ICommandData, ]], ]]



# interface ILockstepEventHandler
       public interface ILockstepEventsHandler
       {
              ushort GetListenInput();
              void Initialize();
              void EarlyInitialize();
              void LateInitialize();
              void Simulate();
              void LateSimulate();
              void Visualize();
              void LateVisualize();
              void GlobalExecute(Command com);
              void RawExecute(Command com);
              void GameStart();
              void Deactivate();
       }

===========


TimePerFrame | 0.02 sec
FramePerStep | 6
