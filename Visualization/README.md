## Design principles for Data Visualization

Matplotlib has two interfaces:

- A functional interface: we use functions to create and modify plots.
- An object-oriented (OO) interface: we use methods to create and modify plots.

We use the functional approach when we call the function like plt.barh().,plt.plot(), plt.scatter(), plt.title(), plt.xlim(), etc.

The functional interface is simpler and easier to use. It comes in handy in the exploratory data visualization workflow, where we need to create graphs fast. But the OO interface offers more power and flexibility in graph editing.

To create a graph using the OO interface, we use the plt.subplots() function, which generates an empty plot and returns a tuple of two objects:

    plt.subplots()
    
    (<Figure size 432x288 with 1 Axes>,
     <matplotlib.axes._subplots.AxesSubplot at 0x7ff15c193850>)

We assign the two objects inside the tuple to variables fig and ax:

    fig, ax = plt.subplots()
    print(type(fig))
    print(type(ax))
    
    <class 'matplotlib.figure.Figure'>
    <class 'matplotlib.axes._subplots.AxesSubplot'>
    

The matplotlib.figure.Figure object acts as a canvas on which we can add one or more plots. The matplotlib.axes._subplots.AxesSubplot object is the actual plot. In short, we have two objects:

- The Figure (the canvas)
- The Axes (the plot; don't confuse with "axis," which is the x- and y-axis of a plot).

To create a bar plot, we use the Axes.bar() method and call plt.show() to display:

    fig, ax = plt.subplots()
    ax.bar(['A', 'B', 'C'],
           [2, 4, 16])
