Necessity for swapping/shifting/rotating motor positions rises from time to time. Before v3.1, we had to either use long wires between FC and ESC to retain the original mapping (which make the build messy) or use custom mmix to change how each motor contributes to attitude adjustment.

With 3.1, we can easily modify motor mappings with `resource` CLI command. See [Betaflight resource mapping](https://github.com/betaflight/betaflight/wiki/Betaflight-resource-remapping) for descriptions of this command.

Here, the most common case of "rotating FC or PDB" is taken as an example.

Suppose you have a board with motor numbers labeled like this:

```
     FRONT
4            2

3            1
     BACK
```

and for some reason, you want to rotate the board 90-degrees clock wise, ending up with
```
     FRONT
3            4

1            2
     BACK
```

Let's fix this.

1. Record the current motor mapping

    Type `resource list` to find out the original mapping.
    ```
# resource list
...
A06: MOTOR 1
A07: MOTOR 2
A11: MOTOR 3
A12: MOTOR 4
...
```

2. Write a diagram with the original pin to motor mapping.

    ```
           FRONT
4(A12)       2(A7)

3(A11)       1(A6)
           BACK
```

3. Rotate the drawing clock wise 90-degrees.

    ```
           FRONT
3(A11)       4(A12)

1(A6)        2(A7)
           BACK
```

4. Remove the old motor position label.

    ```
           FRONT
A11          A12

A6           A7
           BACK
```
This is your actual MCU pin position.

5. Assign new motor position label to the MCU pins.

    ```
           FRONT
4(A11)       2(A12)

3(A6)        1(A7)
           BACK
```
This is your new mapping.

6. The cli resource command for this mapping is:

    ```
resource motor 1 a7
resource motor 2 a12
resource motor 3 a6
resource motor 4 a11
save
```

This is just a example. Please work on your case on your own and verify the results.

Have fun!!!