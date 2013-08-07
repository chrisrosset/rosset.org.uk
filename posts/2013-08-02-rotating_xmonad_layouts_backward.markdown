----
title: Rotating XMonad Layouts Backwards
tags: xmonad haskell blog
----

I've been using XMonad for about a year now and I'm extremely satisfied with
how it works. One small feature that I've been missing from it though is the
ability to move backwards in the layout selection. Sometimes I'll miss the
layout I'm looking and since I've got quite a few layouts it's a bit tedious
to have to through all of them.

The way I've got the regular layout switching set up is:

    ((modm, xK_space ), sendMessage NextLayout)

Unfortunately, there's no `PrevLayout` message -- that would be a pretty
immediate solution to my problem.

My (hackish) solution to this was to define this action in terms of
`NextLayout`. Namely, this is just doing `sendMessage NextLayout` (n-1) times
where n is the number of layouts you have available. So instead of the ideal:

    sendMessage PrevLayout

I've decided to write the following:

    do { replicateM_ (noOfLayouts-1) $ sendMessage NextLayout})

Thus, I needed to get the number of layouts from somewhere.

This is how I had my layouts defined before:

    myLayout = reflectHoriz tiled
           ||| tiled
           ||| Mirror tiled
           ||| noBorders Full
           ||| Grid False
           ||| GridRatio (1) True
           ||| ThreeColMid nmaster delta ratioColumns

Originally, I wanted to throw all these layouts into a list, get the number
by using `length` and combine the layouts using `foldl1 (|||)`. This approach,
unfortunaly, did not work as the layouts are not of the same type.

My, sadly hackish, solution to this problem was to define a function that would
count the layouts as they are combined.

    (|+|) :: (LayoutClass l a, LayoutClass r a) =>
                    (l a, Int) -> r a -> (Choose l r a, Int)
    (a,n) |+| b = (a ||| b, n+1)
    
    myLayout = fst countedLayouts
    noOfLayouts = snd countedLayouts
    countedLayouts = (reflectHoriz tiled, 0)
                  |+| tiled
                  |+| Mirror tiled
                  |+| noBorders Full
                  |+| Grid False
                  |+| GridRatio (1) True
                  |+| ThreeColMid nmaster delta ratioColumns
