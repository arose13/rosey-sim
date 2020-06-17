# Rosey-sim
Causal, Probabilistic and Uncertainty Simulations

## Installation
```bash
pip install rosey-sim
```

## Example
```python
from rosey_sim.sensitivity import Model, Parameter
from tqdm import trange
import matplotlib.pyplot as graph
import seaborn as sns

# Specify function
def profit_func(price, cost):
    return price - cost

# Specify model (NOTE the parameters matches the functions arguments)
profit_model = Model(
    profit_func,
    params=[
        Parameter('cost', [3, 7]),
        Parameter('price', [10, 12])
    ]
)

# Single sample
print(f'${profit_model.sample():.2f}')

# Many single samples
trace = [profit_model.sample() for _ in trange(10000)]
sns.distplot(trace, label='One at a time')

# Many sample immediately
trace = profit_model.sample(10000)
sns.distplot(trace, label='All at once')
graph.show()

# Even nesting is allowed
def random_divide(profit_func, denom):
    return profit_func / denom

final_model = Model(
    random_divide,
    params=[
        profit_model,
        Parameter('denom', [3, 4])
    ]
)
print(final_model.sample())
```
