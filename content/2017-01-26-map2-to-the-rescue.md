Title: Maybe.map2 to the rescue
Date: 2017-01-26 11:20
Category: Programming

(I wrote the following blog post almost one year ago, but never published it. I think now &ndash; with my new website &ndash; the time has come...)

These days I had a problem. I wrote this function:

    :::elm
    sliceSize : Int -> Int -> Int -> Float
    sliceSize slices min max =
      (max - min) / (toFloat slices)

It takes an interval (given by min- and max-value) and a number of slices and than calculates the size of a slice. For example: If the value of min is 20, the value of max is 50 and the number of slices is three, the function will return 10. So far everything is fine. 

Say, this function is called by another function that calculates the min- and max-value from a list of values. My first attempt looked something like this:

    :::elm
    data : List Int
    data =
      [34, 98, 75, 40, 56]


    getSliceSize : Int -> List Int -> Float
    getSliceSize slices data =
      let
        min = 
          List.minimum data 
          |> Maybe.withDefault 0
        max = 
          List.maximum data 
          |> Maybe.withDefault 0
      in
        sliceSize slices min max 

This does not look very elegant and while it is okay in this case, their might be other cases where it is not appropriate to choose 0 as default value for a min- or max-value. So it is time to rethink the problem and try something else. Second solution:

    :::elm
    getSliceSize' : Int -> List Int -> Maybe Float
    getSliceSize' slices data =
      case List.minimum data of
        Just min ->
          case List.maximum data of
            Just max ->
              Just (sliceSize slices min max)
            Nothing ->
              Nothing
        Nothing ->
          Nothing

While this behaves much better it looks awkward and is way too much typing. One thing that is different to our former solution is, that the return-type changed. It is not longer Float, but Maybe Float. This is okay and -- in fact -- a bonus: Now it is not longer implicit how to treat cases, where there is nor min-, neither a max-value and the calling code has to explicitly handle special (error) cases.

There has to be a better solution, I thought, that is shorter and achieves the same. So I looked how this is solved in other languages (Haskell), because I thought that functions on Functors or Monads may be a solution for this. And while that is true, it is much easier to take a look into the Elm docs and what other functions the [Maybe-module](http://package.elm-lang.org/packages/elm-lang/core/3.0.0/Maybe) provides. The final solution:

    :::elm
    getSliceSize'' : Int -> List Int -> Maybe Float
    getSliceSize'' slices data =
      Maybe.map2 
        (sliceSize slices) 
          (List.minimum data) 
          (List.maximum data)

There is no need to handle the Just's or Nothing's yourself, the Maybe.map2 (and of course there are map, map3, map4 and map5) does this for you: If one of the incoming values is Nothing than the result will be Nothing. Else the (Just) values for min and max are taken to compute the result. 

I was very happy when I figured out this elegant solution and I think that this is not obvious when you start writing Elm code. If you think that this is not the best solution or if you have an even better one, than please leave a comment below. 