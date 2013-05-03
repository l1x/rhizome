!https://dl.dropboxusercontent.com/u/174179/rhizome/rhizome.jpg!

Rhizome is a library for visualizing graph and tree structures.

h3. Usage

To include in your project, add this to your `project.clj`:

```clj
[rhizome "0.1.0"]
```

Use of this project requires that "Graphviz":http://www.graphviz.org is installed, which can be checked by running `dot -V` at the command line.  If it's not installed, you can do the following:

| Linux | install `graphviz` using your package manager |
| OS X | "download the installer":http://www.graphviz.org/Download_macos.php |
| Windows |  "download the installer":http://www.graphviz.org/Download_windows.php |

There are two namespaces, `rhizome.dot` and `rhizome.viz`.  The former will take a graph and return a string representation of a Graphviz dot file, the latter takes graphs and renders or displays them.  In practice, you should only need to use `rhizome.viz`.

The core function is `rhizome.viz/view-graph`.  It takes the following keyword arguments:

| *name* | *required?* | *description* |
| `:nodes` | yes | a list of the nodes within the graph |
| `:adjacent` | yes | a function that takes a node, and returns adjacent nodes |
| `:directed?` | no | whether the graph should be rendered as a directed graph, defaults to true |
| `:vertical?` | no | whether the graph should be rendered top-to-bottom, defaults to true |
| `:node->descriptor` | no | takes a node, and returns a map of attributes onto values describing how the node should be rendered |
| `:edge->descriptor` | no | takes the source and destination node, and returns a map of attributes onto values describing how the edge should be rendered |
| `:options` | no | a map of attributes onto values describing how the graph should be rendered |

The rendering attributes described by `:node->descriptor`, `:edge->descriptor`, and `:option` are described in detail "here":http://www.graphviz.org/content/attrs.  The most commonly-used attributes are `label`, which describes the text overlaid on a node or edge, and `shape`, the options for which are described "here":http://www.graphviz.org/content/node-shapes.  For the `:options`, it's sometimes useful to adjust the `dpi`, which controls the size of the image.

An example:

```clj
> (use 'rhizome.viz)
nil
> (def g 
    {:a [:b :c]
	 :b [:c]
	 :c [:a]})
#'g
> (view-graph
    :nodes (keys g)
    :adjacent g
    :node->descriptor (fn [n] {:label (pr-str n)}))
```

![https://dl.dropboxusercontent.com/u/174179/rhizome/example_graph.png]()

While trees are a special case of graphs, using `view-graph` to visualize trees can be a little indirect.  To make this simpler, there's a `view-tree` function, which is modeled after Clojure's `tree-seq` operator.  This function takes three parameters, `branch?`, `children`, and `root`, followed by zero or more of the keyword arguments taken by `view-graph`.  This can make it easy to visualize hierarchical structures:

```clj
> (def t [[1 [2 3]] [4 [5]]])
#'t
> (view-tree sequential? seq t
    :node->descriptor (fn [n] {:label (when (number? n) (str n))}))
```

![https://dl.dropboxusercontent.com/u/174179/rhizome/example_tree.png]()

h3. License

Copyright © 2013 Zachary Tellman

Distributed under the "MIT License":http://opensource.org/licenses/MIT