# PyTorch basic

- Layers:
  As convention, the input of a model or a layer includes a Batch dimension. i.e. the MNIST recognition model expects `(batch * 784)` input.

- callable class:
  Some models are callable by defining a `__call__` method. Then you can call `pred = model(X)`

- `nn.Flatten()`
  Flatten layer, automatically flatten what ever input's shape is given.

- `nn.Sequential()`
  A stack of layers, run one after another (sequential)

- `nn.Linear()`

- `nn.ReLU`

- `def forward()`
  Conventionally, pytorch suppose each class would have a `forward()` method. If not implemented, runtime error will occurs.

- `.to(device)`
  `.to(device)` moves (or copies) the underlying **tensor** storage of an object from CPU memory to GPU memory (or vice versa), for objects that own or contain tensors.

  - Tape-Based autograd: One of the key features of PyTorch’s autograd is that it builds the computation graph dynamically, on a “pay-as-you-go” basis. You can write any logic in `forward()` function, even `if-else`. PyTorch will record whatever the graph is and do `backward()`

- `.forward()`
  `nn.Module.__call__ = `

- Batch tensor
  It is not recommended to user `for` loop in batch loop.
  Instead, you should use matrix computation to do so. 
  ```python
  # reference: broadcasting mechanism
  # x.shape
  # >>> (B, in_dim)
  # W.shape
  # >>> (in_dim, out_dim)
  y = x @ W # y.shape: (B, out_dim)
  ```

## Auto grad
- Intuition
  `loss = loss_fn(pred, y)` is a tensor. `loss` is formulated and calculated first, meanwhile the calculation history (graph) is recorded. Then do `.backward()`, the derivative of tensor items that involved in `loss`'s calculation will be stored in those tensors. 
  `optimizer` is a stateful (not partial derivatives, but some 'meta' information or updating stats) controller that owns references to a set of parameter tensors and defines a rule for updating their underlying data using gradients. 
  Call `optimizer.step()`, each parameter that optimizer refers to will be updated according to rules.

- Back Propagation
  The graph record the variable and computation operation in each step. When you call `loss.backward()` it traverses the graph in reverse order, starting from loss, and calculates the derivatives for each vertex. Whenever a leaf is reached, the calculated derivative for that tensor is stored in its .grad attribute.

- Batch training / Mini batch
  In batch training, we usually run a batch sample with the same model parameters and calculate the gradients, then accumulate, then updating the model at once.

- Gradient accumulation:
  When the mini-batch size becomes too large, the intermediate activations required during the backward pass may not fit in memory, because backpropagation needs to store these activations from the forward pass.
  To avoid excessive memory usage while still benefiting from a larger effective batch size (which can lead to more stable and accurate training), we split a large batch into several smaller mini-batches.
  For each mini-batch, we run backward() but do not update the weights and do not call zero_grad(). This allows the gradients stored in .grad to accumulate across multiple backward passes.
  After several mini-batches, we then perform a single weight update using optimizer.step() and finally clear the gradients.

 > https://stackoverflow.com/questions/62067400/understanding-accumulated-gradients-in-pytorch



- `.grad_fn`
- `.grad`: return the 

- Turning on/off autograd
  - `requires_grad=False`, No record at all
  - `.no_grad()`, Temporarily
  - `.detach()`, return a copt without and computation history


- In-place operation is not allowed
  You can only compute a tensor and assign to another variable, otherwise will be runtime error

- Computation of Chain Rule
  - Jacobian matrix (derivative matrix)
    The derivative value of leaf is given by derivative-function-matrix times value of upper layer (`J*v`)

# Deep Learning Basic 

- epoch

- Drop out layers
  Dropout is a regularization layer used during training to reduce overfitting by randomly turning off (zeroing) a fraction of neurons on each forward pass.
  In pytorch, dropout layer is affected by model's training/eval mode 

- 

# Home server
- `pm2` daemon

# Python Basic

- `__init__.py`
  `my_package` is a directory with an `__init__.py`. If you `import my_package`, a module object `my_package` is created, and `__init__.py` is executed. 
  If you `from my_package import my_module`, the module object is created.
  It is recommended to explicitly declare the modules in the package in `__init__.py` to avoid confusion.


## MVVM model view-model view

Just like MVC, MVVM is also a design pattern for UI applications

- The view is responsible for defining the structure, layout, and appearance of what the user sees on screen. Ideally, each view is defined in XAML, with a limited code-behind that does not contain business logic. 

- View model is the abstract model of View UI. It implements properties and commands to which the view can data bind to, and notifies the view of any state changes through change notification events.
  - User filling form, click button. These actions essentially trigger viewmodel's function or set its data.
  - Viewmodel does not have access to any reference in view. But it can notify view to update its status. (like Qt emit signal)
  - viewmodel should be asynchronous

- Model classes are non-visual classes that encapsulate the app's data. Therefore, the model can be thought of as representing the app's domain model.
  - Usually includes a data model along with business and validation logic.

# C++ Basic

- final class:
  Not allowed inheritance

## Pass-by-value + move semantics in constructors

In C++, passing an object by value means the callee receives its own copy of the argument, independent of the caller’s object. This copy lives within the function’s scope and can be freely modified without affecting the original.

- When a constructor takes a parameter by value (e.g. `std::vector<size_t> shape`), the caller’s object is copied into a local parameter object. This copy is the only place where element-wise copying may occur.
  
- Inside the constructor’s member initializer list, `std::move(shape)` enables move semantics, transferring the internal resources of this local copy to the class member instead of copying them again.
  - For containers such as `std::vector`, this typically means transferring ownership of the underlying memory buffer rather than duplicating its contents.
  - The moved-from parameter remains in a valid but unspecified state and should no longer be relied upon.

- This pattern combines safety and efficiency:
  - The public interface is simple and safe for callers.
  - Internally, the constructor avoids redundant deep copies and achieves near O(1) transfer cost.

This “pass-by-value then move” pattern is a common idiom in modern C++ for initializing resource-owning members efficiently.


## Forward Declaration

## Public / Private / Protected Inheritance

## When to use smart pointer

## virtual function and override

## Generic type class: template typename

## Data class design: view + storage

## pointer, reference, value, Smart pointer


- Reference:
  Its an alias of the object.  
  In function parameter, `Tensor::backward(T& t)` means that, you pass a variable `my_tensor` to the function (`backward(my_tensor)`), and in the scope of the function, `my_tensor` is called `t`. Doing modification on `t`, is just as on object `my_tensor`. But if you add `(const T& t)`, then the object is immutable. The semantic meaning is: `t` is an immutable (const) reference of type `T`

  - PS: right value operator `T* p = &t`. In this case `&t` return the address value

- Pointer:
  Pointer stores an address value  
  `f(const T* t)`: Same as reference, the object `my_tensor` per se is immutable


  `f(T* const t)`: Different from reference, the pointer `t` per se is immutable. In the scope of `f`, you cannot change the value `t` pointing at.

- When to use smart pointer?

## Safety Outline

### Undefined Behavior

### Memory safety

### Lifetime Safety

### Type Safety

### API safety

### Solution

#### avoid raw pointer

#### RAII

#### Use library whenever possible


# Java

# Data Structure and Algorithm

## Two sum, three sum, 
- Two pointer
> leetcode 15, 167

  For two sum you can use two pointers to select a window, and update the boundary based on the sum in the window

  You can reduce the three sum problem to a two pointer problem by fixing the first pointer. Which reduce the time from O(n^3) -> O(n^2)


## Maxwater, Candy distribution
- Dynamic programming
> leetcode 42, 135

  The key is to maintain two intermediate state array. Each guarantees a partial valid solution (ensure all kids satisfy part of the constrains - left/right). Then . Time complexity O(3\*n)


- Two pointer
> leetcode 11  

  You have two pointers, left and right. Maintain the global maxWater. And each time shrink the window, and test if it perform better `maxWater`.
  
## Sliding window
> Leetcode 209

  If `sum >= target` shrink the window, until the condition doesn't meet anymore. Then extend the window until condition meet.

  Usually requires two while loops.





