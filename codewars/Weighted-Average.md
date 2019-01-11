# Elections: Weighted Average
## Detail
Description:
Many websites use weighted averages of various polls to make projections for elections. They’re weighted based on a variety of factors, such as historical accuracy of the polling firm, sample size, as well as date(s). The weights, in this kata, are already calculated for you. All you need to do is convert a set of polls with weights, into a fixed projection for the result.

Task:
Your job is to convert an array of candidates (variable name `candidates`) and an array of `polls` (variable name polls), each poll with two parts, a result and a weight, into a guess of the result, with each value rounded to one decimal place, through use of a weighted average. Weights can be zero! Don't worry about the sum not totalling 100. The final result should be a hash in Ruby and Crystal, dictionary in Python, or object in JS in the format shown below:
```
{ 
  "<candidate name 1>": "<candidate's projected percent, rounded to nearest tenth>",
  "<candidate name 2>": "<candidate's projected percent, rounded to nearest tenth>",
  ...
}
```
The input should not be modified.

Calculation for projections:
```
[(poll1 * weight1) + (poll2 * weight2) + ...] / (weight1 + weight2 + ...)
```
An example:
```
candidates = ['A', 'B', 'C']

poll1res = [20, 30, 50]
poll1wt = 1
poll1 = [poll1res, poll1wt]

poll2res = [40, 40, 20]
poll2wt = 0.5
poll2 = [poll2res, poll2wt]

poll3res = [50, 40, 10]
poll3wt = 2
poll3 = [poll3res, poll3wt]

polls = [poll1, poll2, poll3]

predict(candidates, polls)
#=> {
     'A': 40,
     'B': 37.1,
     'C': 22.9
    }

# because...

candidate 'A' weighted average
  = ((20 * 1) + (40 * 0.5) + (50 * 2)) / (1 + 0.5 + 2)
  = (20 + 20 + 100) / 3.5
  = 140 / 3.5
  = 40

candidate 'B' weighted average
  = ((30 * 1) + (40 * 0.5) + (40 * 2)) / (1 + 0.5 + 2)
  = (30 + 20 + 80) / 3.5
  = 130 / 3.5
  = 37.142857...
  ≈ 37.1 (round to nearest tenth)

candidate 'C' weighted average
  = ((50 * 1) + (20 * 0.5) + (10 * 2)) / (1 + 0.5 + 2)
  = (50 + 10 + 20) / 3.5
  = 80 / 3.5
  = 22.857142...
  ≈ 22.9 (round to nearest tenth)
```

## Answer
**My Solution**
```
function predict(candidates, polls) {
  return candidates.reduce((result,current,index)=>{
    result[current]= parseFloat((polls.reduce((a,b)=>a+b[0][index]*b[1],0)/polls.reduce((a,b)=>a+b[1],0)).toFixed(1));
    return result;
  },{});
}
```
**Best Solutions**
```
function predict(candidates, polls) {
  let res = {};
  candidates.forEach((v, i) => {
    res[v] = +(polls.reduce((s, v) => s + v[0][i] * v[1], 0) / polls.reduce((s, v) => s + v[1], 0)).toFixed(1);
  });
  return res;
}
```