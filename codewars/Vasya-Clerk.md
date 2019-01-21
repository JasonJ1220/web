# Vasya - Clerk
## Detail
The new "Avengers" movie has just been released! There are a lot of people at the cinema box office standing in a huge line. Each of them has a single `100, 50 or 25` dollars bill. An "Avengers" ticket costs `25 dollars`.

Vasya is currently working as a clerk. He wants to sell a ticket to every single person in this line.

Can Vasya sell a ticket to each person and give the change if he initially has no money and sells the tickets strictly in the order people follow in the line?

Return YES, if Vasya can sell a ticket to each person and give the change with the bills he has at hand at that moment. Otherwise return `NO`.

Examples:
```
tickets([25, 25, 50]) // => YES 
tickets([25, 100]) // => NO. Vasya will not have enough money to give change to 100 dollars
tickets([25, 25, 50, 50, 100]) // => NO. Vasya will not have the right bills to give 75 dollars of change (you can't make two bills of 25 from one of 50)
```
## Answer
**My Solution**
```
function tickets(peopleInLine) {
    var change = [],
        result = "YES";
    
    peopleInLine.forEach(function (item) {
        if (item === 25) {
            change.push(25);
        } else if (item === 50) {
            var index25 = change.indexOf(25);
            if (index25 > -1) {
                change.splice(index25, 1);
                change.push(50);
            } else {
                result = "NO";
            }
        } else {
            var index50 = change.indexOf(50);
            var index25 = change.indexOf(25);
            var count25 = change.filter(function (item) {
                return item === 25;
            }).length;
            if (index50 > -1 && index25 > -1) {
                change.splice(index50, 1);
                change.push(100);
                index25 = change.indexOf(25);
                change.splice(index25, 1);
            } else if (index50 === -1 && count25 >= 3) {
                var i = 3;
                while (i > 0) {
                    index25 = change.indexOf(25);
                    change.splice(index25, 1);
                    i--;
                }
            } else {
                result = "NO";
            }
        }
    });
    console.log(change);
    return result;
}
```
**Best Solutions**
```
function Clerk(name) {
  this.name = name;
  
  this.money = {
    25 : 0,
    50 : 0,
    100: 0 
  };
  
  this.sell = function(element, index, array) {
    this.money[element]++;

    switch (element) {
      case 25:
        return true;
      case 50:
        this.money[25]--;
        break;
      case 100:
        this.money[50] ? this.money[50]-- : this.money[25] -= 2;
        this.money[25]--;
        break;
    }
    return this.money[25] >= 0;
  };
}

function tickets(peopleInLine){
  var vasya = new Clerk("Vasya");
  return peopleInLine.every(vasya.sell.bind(vasya)) ? "YES" : "NO";
}
```