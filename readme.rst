Database Preferences and Skyline Computation in Python
======================================================

Routines to select and visualize the maxima for a given strict partial 
order. This especially includes the computation of the Pareto 
frontier, also known as (Top-k) Skyline operator, and some 
generalizations (database preferences).

This the Python porting of the rPref package.


Examples
--------


# Simple preference selection
# ---------------------------

# include package
import pypref as p

# load mtcars data set given in pypref (motor trends data set from R)
mtcars = p.get_mtcars()

# preference for cars with minimal fuel consumption (high mpg value) and high power
pref = p.high("mpg") * p.high("hp")

# select optimal cars according to this preference
pref.psel(mtcars)


# Simple visualization example
# ----------------------------

# include matplotlib
import matplotlib.pyplot as plt

def plot_levels(dataset, pref):

  # get level values for all tuples from the data set
  res = pref.psel(dataset, top = len(dataset))
  
  # plot each level front line in a different color
  for level in range(1, res['_level'].max() + 1):
    pts = res.loc[res['_level'] == level].sort_values("mpg")
    plt.step(pts['mpg'], pts['hp'], 'o', label = "Level " + str(level))

  # show legend and plot
  plt.legend()
  plt.show()
  
# show level plot for data set and preference as given above
plot_levels(mtcars, pref)