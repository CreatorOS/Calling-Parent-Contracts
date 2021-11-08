# Calling Parent Contracts

Let's create a Parent contract `A`:
Here, we are using an `event` `Log` to emit logs.
In our case, this will be useful for tracing function calls.

```
contract A {
    event Log(string message);

    function foo() public virtual {
        emit Log("A.foo called");
    }

    function bar() public virtual {
        emit Log("A.bar called");
    }
}
```

## Calling parent contract functions

Parent contracts can be called directly, or by using the keyword `super`.

By using the keyword `super`, all of the immediate parent contracts will be called.

Code up these contracts:

```
contract B is A {
    function foo() public virtual override {
        emit Log("B.foo called");
        A.foo();
    }

    function bar() public virtual override {
        emit Log("B.bar called");
        super.bar();
    }
}

contract C is A {
    function foo() public virtual override {
        emit Log("C.foo called");
        A.foo();
    }

    function bar() public virtual override {
        emit Log("C.bar called");
        super.bar();
    }
}
```

## Testing the order of function calls

Code this last contract and then we will test the order of function calls:

```
contract DCPC is B, C {

    function foo() public override(B, C) {
        super.foo();
    }

    function bar() public override(B, C) {
        super.bar();
    }
}
```

Hit `Run` and then check the output logs.

- Check log of call `DCPC.foo()` in 2nd test output.
  - Although DCPC inherits A, B and C, it only called C and then A.
- Check log of call `DCPC.bar()` in 3rd test output.
  - DCPC called C, then B, and finally A.
  - Although super was called twice (by B and C) it only called A once.
