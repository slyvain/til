# Remove <Unknown Project> from Test Explorer

![Unknown Project in Test Explorer](../assets/vs-unknown-project.png?raw=true)

Some remnant unit tests still appear in the Test Explorer. Most likely those units have been renamed or deleted, but VS keeps them under the `<Unknown Project>` project.
And for some reason, in this case, the tests pass! :satisfied:

How to remove it:

1. Close Visual Studio
2. Navigate to the solution's root directory
3. Delete the `*.testlog` files in `.vs\<solution name>\v16\TestStore\<number>`
4. Alternatively, simply delete the `TestStore` directory