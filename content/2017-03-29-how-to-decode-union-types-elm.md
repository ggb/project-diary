Title: How to decode union types in Elm
Date: 2017-03-29 11:20
Category: Programming

Today I came across a problem regarding JSON decoding in Elm: How do you decode a string (e. g. nested in some object) to a union type in Elm? Say you have this

    :::elm
    someJsonString = 
      "{\"f\": \"Done\"}" 

and after decoding it should be this:

    :::elm
    type Status 
      = Done
      | Open
      | Failed

    type alias Stuff = 
      { f: Status }

    someParsedJson = 
      {f = Done}

I was surprised to not find this covered in the Elm guide and while there are some [Stackoverflow-](http://stackoverflow.com/questions/40826911/elm-how-to-json-decode-a-union-type-with-a-single-typeconstructor) and blog-posts on how to decode tagged unions, I found nothing about plain union types. So if you come across the same issue, here is the solution:

    :::elm
    statusDecoder =
      let
        statusHelper s =
          case s of
            "Done" -> Done
            "Open" -> Open
            _ -> Failed
            
      in
        map statusHelper string


    stuffDecoder = 
      map Stuff
        (field "f" statusDecoder)

Find the whole example code in this [gist](https://gist.github.com/ggb/222385d8a2b734616a7cfaaa83b2f580) and please let me know if there are other ways to tackle this problem.