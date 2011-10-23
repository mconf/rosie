# Rosie

![Rosie the Riveter](http://upload.wikimedia.org/wikipedia/commons/thumb/1/12/We_Can_Do_It%21.jpg/220px-We_Can_Do_It%21.jpg)

Rosie is a factory for building JavaScript objects, mostly useful for setting up test data. It is inspired by [factory_girl](https://github.com/thoughtbot/factory_girl).

## Usage

Define your factory, giving it a name and optionally a constructor function:

    var Game = function(attrs) {
      for(var attr in attrs) {
        this[attr] = attrs[attr];
      }
    };

    Factory.define('game', Game)
      .sequence('id')
      .attr('is_over', false)
      .attr('created_at', function() { return new Date(); })
      .attr('random_seed', function() { return Math.random(); })
      .attr('players', function() {
        return [
          Factory.attributes('player'),
          Factory.attributes('player')
        ];
      });

    Factory.define('player')
      .sequence('id')
      .sequence('name', function(i) { return 'player' + i; });

You can also pass a hash of functions or attributes in bulk like so:

    var function_hash = {
      get_score: function() {
        return 1337;
      },

      get_player_name: function() {
        return "Player 1";
      }
    };

    var attributes_hash = {
      arena: {
        size: 10000,
        location: "San Francisco, CA"
      },

      players: 8
    }

    Factory.define('game', Game)
      .set_funcs(function_hash)
      .set_attrs(attributes_hash);

Now you can build an object, passing in attributes that you want to override:

    var game = Factory.build('game', {is_over:true});

Which returns an object that looks roughly like:

    {
      id:           1,
      is_over:      true,   // overriden when building
      created_at:   Fri Apr 15 2011 12:02:25 GMT-0400 (EDT),
      random_seed:  0.8999513240996748,
      players: [
                    {id: 1, name:'Player 1'},
                    {id: 1, name:'Player 2'}
      ]
    }

For a factory with a constructor, if you want just the attributes:

    Factory.attributes('game') // return just the attributes


We can also define full objects without a object template by defining the methods yourself...

    var game_functions = {
      get_score: function() {
        return this.game_score;
      },

      set_score: function(val) {
        this.game_score = val;
      }
    };

    var game_attributes = {
      is_over: false,
      game_score: 10
    };

    Factory.define('game')
      .set_funcs(game_functions)
      .set_attrs(game_attributes);


Then we can construct our object by building an object..

    var game = Factory.build('game', {is_over:true});

## Credits

Thanks to [Daniel Morrison](http://twitter.com/danielmorrison/status/58883772040486912) for the name and [Jon Hoyt](http://twitter.com/jonmagic) for inspiration and brainstorming the idea.

Explicit object definition and creation contributed by TrueCar.com:
Mishkin Faustini - mfaustini@truecar.com
Satya Phanse - sphanse@truecar.com