Core
====

The Core module is used to add extended functionality to objects without
extending `Object.prototype` directly.

    module.exports = (I={}, self={}) ->
      extend self,

External access to instance variables. Use of this property should be avoided
in general, but can come in handy from time to time.

>     #! example
>     I =
>       r: 255
>       g: 0
>       b: 100
>
>     myObject = Core(I)
>
>     [myObject.I.r, myObject.I.g, myObject.I.b]

        I: I

Generates a public jQuery style getter / setter method for each `String` argument.

>     #! example
>     myObject = Core
>       r: 255
>       g: 0
>       b: 100
>
>     myObject.attrAccessor "r", "g", "b"
>
>     myObject.r(254)

        attrAccessor: (attrNames...) ->
          attrNames.forEach (attrName) ->
            self[attrName] = (newValue) ->
              if arguments.length > 0
                I[attrName] = newValue

                return self
              else
                I[attrName]

          return self

Generates a public jQuery style getter method for each String argument.

>     #! example
>     myObject = Core
>       r: 255
>       g: 0
>       b: 100
>
>     myObject.attrReader "r", "g", "b"
>
>     [myObject.r(), myObject.g(), myObject.b()]

        attrReader: (attrNames...) ->
          attrNames.forEach (attrName) ->
            self[attrName] = ->
              I[attrName]

          return self

Extends this object with methods from the passed in object. A shortcut for Object.extend(self, methods)

>     I =
>       x: 30
>       y: 40
>       maxSpeed: 5
>
>     # we are using extend to give player
>     # additional methods that Core doesn't have
>     player = Core(I).extend
>       increaseSpeed: ->
>         I.maxSpeed += 1
>
>     player.increaseSpeed()

        extend: (objects...) ->
          extend self, objects...

Includes a module in this object. A module is a constructor that takes two parameters, `I` and `self`

>     myObject = Core()
>     myObject.include(Bindable)

>     # now you can bind handlers to functions
>     myObject.bind "someEvent", ->
>       alert("wow. that was easy.")

        include: (modules...) ->
          for Module in modules
            Module(I, self)

          return self

      return self

Helpers
-------

Extend an object with the properties of other objects.

    extend = (target, sources...) ->
      for source in sources
        for name of source
          target[name] = source[name]

      return target
