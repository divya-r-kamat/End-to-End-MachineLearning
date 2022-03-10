## Design principles for Data Visualization

When we design graphs (and anything in general), we need design principles to guide us. Design principles help us in two ways:

- They generate design options.
- They help us choose among those options.

One design principle is familiarity. For example, if we need to visually present a frequency distribution, familiarity gives us a few options: a histogram and a box plot (let's assume our audience is only familiar with these two). Our audience, however, is more familiar with histograms, so we choose a histogram for our presentation.

The next design principle has to do with maximizing data elements on a graph. Generally, a graph has three elements:

- Data elements: the numbers and the categories visually represented and the relationships between them.
- Structural elements: the axes, the ticks, the legend, the grid, etc.
- Decorations: extra colors, shapes, artistic drawings etc.

Maximizing the data elements ensures the audience's attention is on the data — not on structure or decorations. 

## Visualization using Matplotlib

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


To change the proportions, we can use the figsize parameter inside the plt.subplots(figsize=(width, height)) function:

        fig, ax = plt.subplots(figsize=(3, 5))
        


To create a bar plot, we use the Axes.bar() method and call plt.show() to display:

    fig, ax = plt.subplots()
    ax.bar(['A', 'B', 'C'],
           [2, 4, 16])


To remove the axes (also called spines), we can use the Axes.spines[position].set_visible(bool) method, where position is a string indicating the location of the axis: 'left', 'right', 'top', and 'bottom'. For instance, this is how we remove the left and the bottom spines.

        fig, ax = plt.subplots()
        ax.bar(['A', 'B', 'C'],
               [2, 4, 16])
        ax.spines['left'].set_visible(False)
        ax.spines['bottom'].set_visible(False)
        plt.show()
        
If we want to remove all the spines, the fastest way is to use a for loop:

        fig, ax = plt.subplots()
        ax.bar(['A', 'B', 'C'],
               [2, 4, 16])

        for location in ['left', 'right', 'bottom', 'top']:
            ax.spines[location].set_visible(False)

        plt.show()
        
To remove the ticks, we can use the Axes.tick_params(bottom, top, left, right) method. Below, we only need to remove the left and bottom ticks:

        fig, ax = plt.subplots()
        ax.bar(['A', 'B', 'C'],
               [2, 4, 16])

        ax.tick_params(bottom=False, left=False)
        plt.show()
        
        
To make the bars less thick, we can use the height parameter inside the Axes.barh() method. The height parameter has a default value of 0.8. Below, we exaggerate a little for the sake of the example and make the bars very thin, with height=0.1.

        fig, ax = plt.subplots(figsize=(4.5, 6))
        ax.barh(top20_deathtoll['Country_Other'],
                top20_deathtoll['Total_Deaths'],
                height=0.1)
        plt.show()
        
To remove some of the x-tick labels, we use the Axes.set_xticks method. Below, we only keep the labels 0, 100000, 200000, and 300000.

        fig, ax = plt.subplots(figsize=(4.5, 6))
        ax.barh(top20_deathtoll['Country_Other'],
                top20_deathtoll['Total_Deaths'])
        ax.set_xticks([0, 100000, 200000, 300000])
        plt.show()
