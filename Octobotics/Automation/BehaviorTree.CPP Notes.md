

## What is a Behavior Tree?

A **Behavior Tree (BT)** is a decision-making architecture used in:

- Robotics
- Game AI  
- Autonomous systems


It controls execution flow using:

- Conditions
- Actions
-  Control logic

---

# Core Concept

Every node returns a status:

```cpp
BT::NodeStatus
```

Possible states:

```cpp
SUCCESS
FAILURE
RUNNING
IDLE
```

---

# Basic Structure

```text
Root
 └── Control Node
      ├── Condition Node
      ├── Action Node
      └── Decorator Node
```

---

# Node Categories

|Type|Purpose|
|---|---|
|Action Node|Perform task|
|Condition Node|Check condition|
|Control Node|Control execution flow|
|Decorator Node|Modify child behavior|
|Root Node|Entry point|

---

# 1. TreeNode (Base Class)

All nodes inherit from:

```cpp
BT::TreeNode
```

Provides:

- tick()
- halt()
- status handling
- ports
- blackboard access

---

# 2. Action Nodes

Used to perform actual tasks.

Examples:

- Move robot
- Open gripper
- Plan path
- Print log

---

## 2.1 SyncActionNode

```cpp
BT::SyncActionNode
```

### Characteristics

- Executes in one tick
- Fast operation
- No continuous RUNNING state

### Returns

```cpp
SUCCESS
FAILURE
```

### Example Use

- Logging
- Variable update
-  Quick calculations

---

## Example

```cpp
class HelloNode : public BT::SyncActionNode
{
public:

    HelloNode(const std::string& name,
              const BT::NodeConfiguration& config)
        : BT::SyncActionNode(name, config)
    {}

    BT::NodeStatus tick() override
    {
        std::cout << "Hello" << std::endl;

        return BT::NodeStatus::SUCCESS;
    }
};
```

---

# 2.2 StatefulActionNode

```cpp
BT::StatefulActionNode
```

Used for:

- Long-running tasks
- Navigation
- Motion control    

---

## Lifecycle Functions

### onStart()

Called first time.

```cpp
BT::NodeStatus onStart()
```

---

### onRunning()

Called repeatedly while RUNNING.

```cpp
BT::NodeStatus onRunning()
```

---

### onHalted()

Called when interrupted.

```cpp
void onHalted()
```

---

## Typical Flow

```text
Tick 1 -> onStart()
Tick 2 -> onRunning()
Tick 3 -> onRunning()
Done   -> SUCCESS
```

---

# 2.3 AsyncActionNode

```cpp
BT::AsyncActionNode
```

Runs task in separate thread.

Useful for:

- Blocking operations
- Heavy computation

---

# 2.4 CoroActionNode

```cpp
BT::CoroActionNode
```

Coroutine-based node.

Allows:

- Pause execution
- Resume later

Advanced usage.

---

# 3. Condition Node

```cpp
BT::ConditionNode
```

Used only to check condition.

---

## Returns

```cpp
SUCCESS -> condition true
FAILURE -> condition false
```

Usually does NOT return RUNNING.

---

## Examples

- Battery OK?
- Obstacle detected?
- Target visible?

---

# Example

```cpp
class BatteryOK : public BT::ConditionNode
```

---

# 4. Control Nodes

Control child execution.

---

# 4.1 Sequence Node

```cpp
BT::SequenceNode
```

Equivalent to logical AND.

---

## Behavior

Executes children left → right.

Stops when:

- child FAILS
- child RUNNING

Returns SUCCESS only if all succeed.

---

## Example

```text
Sequence
 ├── CheckBattery
 ├── PlanPath
 └── MoveRobot
```

---

# Sequence Logic

|Child Result|Sequence Result|
|---|---|
|SUCCESS|Continue|
|FAILURE|Stop + FAILURE|
|RUNNING|Stop + RUNNING|

---

# 4.2 Fallback Node (Selector)

```cpp
BT::FallbackNode
```

Equivalent to logical OR.

---

## Behavior

Try children until one succeeds.

---

## Example

```text
Fallback
 ├── OpenDoor
 └── JumpWindow
```

---

# Fallback Logic

|Child Result|Fallback Result|
|---|---|
|SUCCESS|Stop + SUCCESS|
|FAILURE|Try next child|
|RUNNING|Stop + RUNNING|

---

# 4.3 ReactiveSequence

```cpp
BT::ReactiveSequence
```

Re-checks conditions continuously.

Important for robotics.

---

## Example

```text
ReactiveSequence
 ├── BatteryOK
 └── MoveRobot
```

If battery becomes low:

- movement interrupted immediately
    

---

# 4.4 ReactiveFallback

Continuously checks higher priority conditions.

Useful for:

- emergency stop
    
- obstacle monitoring
    

---

# 4.5 Parallel Node

```cpp
BT::ParallelNode
```

Runs multiple children simultaneously.

---

## Example

```text
Parallel
 ├── Navigate
 └── MonitorBattery
```

---

# 5. Decorator Nodes

Decorator has ONE child.

Modifies child behavior.

---

# 5.1 Inverter

Swaps result.

```text
SUCCESS -> FAILURE
FAILURE -> SUCCESS
```

---

# Example

```text
Inverter
 └── BatteryLow
```

Means:

- if battery low = FAILURE
    
- inverter returns SUCCESS
    

---

# 5.2 Retry Node

Retries child execution.

---

## Example

```text
Retry(3)
 └── ConnectServer
```

---

# 5.3 Repeat Node

Repeats child multiple times.

---

## Example

```text
Repeat(10)
 └── Patrol
```

---

# 5.4 Timeout Node

Stops child after timeout.

---

# 6. Ports

Ports are node inputs and outputs.

---

# Input Port

```cpp
BT::InputPort<int>("target")
```

---

# Output Port

```cpp
BT::OutputPort<std::string>("result")
```

---

# Example

```cpp
static BT::PortsList providedPorts()
{
    return {
        BT::InputPort<std::string>("name")
    };
}
```

---

# Access Input

```cpp
auto name = getInput<std::string>("name");
```

---

# XML Example

```xml
<HelloNode name="Priyanshu"/>
```

---

# 7. Blackboard

Shared memory between nodes.

Stores:

- variables
    
- targets
    
- states
    

---

# Example

```text
Condition writes target
Action reads target
```

---

# 8. Registering Custom Node

```cpp
factory.registerNodeType<HelloNode>("HelloNode");
```

XML tag:

```xml
<HelloNode/>
```

---

# 9. XML Tree Example

```xml
<root BTCPP_format="4">

    <BehaviorTree ID="MainTree">

        <Sequence>

            <BatteryOK/>
            <MoveRobot/>

        </Sequence>

    </BehaviorTree>

</root>
```

---

# Internal Class Hierarchy

```text
TreeNode
 ├── ActionNodeBase
 │     ├── SyncActionNode
 │     ├── StatefulActionNode
 │     ├── AsyncActionNode
 │     └── CoroActionNode
 │
 ├── ConditionNode
 │
 ├── ControlNode
 │     ├── SequenceNode
 │     ├── FallbackNode
 │     ├── ParallelNode
 │     └── ReactiveSequence
 │
 └── DecoratorNode
       ├── Inverter
       ├── RetryNode
       ├── RepeatNode
       └── TimeoutNode
```

---

# Most Important Robotics Nodes

|Node|Importance|
|---|---|
|Sequence|Very High|
|Fallback|Very High|
|ReactiveSequence|Critical|
|StatefulActionNode|Critical|
|ConditionNode|Critical|

---

# Common Robotics Example

```text
ReactiveSequence
 ├── BatteryOK
 ├── ObstacleFree
 └── MoveRobot
```

Robot moves only if:

- battery OK
    
- no obstacle
    

Otherwise movement stops immediately.

---

# Recommended Learning Order

1. SyncActionNode
2. ConditionNode
3. Sequence
4. Fallback
5. Ports
6. Blackboard
7. StatefulActionNode
8. ReactiveSequence
9. Decorators
10. Async nodes

---

# Important Files in Repository

Inside:

```text
examples/
```

Study:

```text
t01_build_your_first_tree.cpp
t02_basic_ports.cpp
t03_generic_ports.cpp
t04_reactive_sequence.cpp
t05_crossdoor.cpp
```

`t05_crossdoor.cpp` is one of the best examples for understanding real BT execution flow.