#Selecting all faces that are connected - MEL code

This is a really brutish way to select all faces connected to an object:

    for ($i = 0; $i < 55; $i++)
        GrowPolygonSelectionRegion;
    
The problem with this approach is that it depends on getting the right amount of iterations to select all the mesh which means that it is possible that with objects with more faces it will not select all faces.

So how do we fix this?

Let's start by finding what happens when you hit Grow. After you hit grow the console shows this:

    select `ls -sl`;PolySelectTraverse 1;select `ls -sl`;
    
Sure enough this works when you copy and paste it onto the command bar.
This is an expression formed by five commands.

    select `ls -sl`;

is formed by two commands, select which selects an object and ls -sl which returns the current selection name. This is unimportant for what we want to do.

    PolySelectTraverse 1;

This is the important command because it will expand the selection. Yet we don't know what that 1 means, it sounds likely that it will expand the selection by 1 face but this is not right at all. If we look the .mel file for PolySelectTraverse we find that it calls a different command:

    polySelectConstraint -pp $traversal -t 0x0001;

This means that the function is actually calling a different command. Going to the polySelectConstraint
