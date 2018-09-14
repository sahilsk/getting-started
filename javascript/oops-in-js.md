# Classis method of using prototype

## Key takeways

- use `call` in child class constructor. eg. ` Animal.call(this, name, energy)`
- override base class prototype eg.  `Dog.prototype = Object.create(Animal.prototype)`. This will help to delegate lookups to parent class if the child class doesn't have the called method.

``` js

function Animal (name, energy) {
  this.name = name
  this.energy = energy
}

Animal.prototype.eat = function (amount) {
  console.log(`${this.name} is eating.`)
  this.energy += amount
}

Animal.prototype.sleep = function (length) {
  console.log(`${this.name} is sleeping.`)
  this.energy += length
}

Animal.prototype.play = function (length) {
  console.log(`${this.name} is playing.`)
  this.energy -= length
}

function Dog (name, energy, breed) {
  Animal.call(this, name, energy)

  this.breed = breed
}

Dog.prototype = Object.create(Animal.prototype)

Dog.prototype.bark = function () {
  console.log('Woof Woof!')
  this.energy -= .1
}

const charlie = new Dog('Charlie', 10, 'Goldendoodle')
console.log(charlie.constructor)
```


# Using ES6 classes


## Key takeways
- use `super` in the constructor to call parent class with arguments.
- To create child class use this: `class Subclass extends Baseclass {}`

``` js
class Animal {
  constructor(name, energy) {
    this.name = name
    this.energy = energy
  }
  eat(amount) {
    console.log(`${this.name} is eating.`)
    this.energy += amount
  }
  sleep() {
    console.log(`${this.name} is sleeping.`)
    this.energy += length
  }
  play() {
    console.log(`${this.name} is playing.`)
    this.energy -= length
  }
}

class Dog extends Animal {
  constructor(name, energy, breed) {
    super(name, energy) // calls Animal's constructor

    this.breed = breed
  }
  bark() {
    console.log('Woof Woof!')
    this.energy -= .1
  }
}
```
