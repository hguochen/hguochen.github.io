---
title: "Kruskal's Algorithm"
layout: post
date: 2014-06-17 00:02:54
comments: true
categories: [programming, greedy algorithm]
---
Given six cities and the costs(in millions of dollars) of rebuilding roads between them. The road commission has decided to rebuild enough roads so that each pair of cities will be connected, either directly or by going through other cities, by rebuilt roads. Find a minimum cost set of roads meeting its criterion.
![city roads](http://i1113.photobucket.com/albums/k508/houguochen/Screenshot2014-06-17004156.png =x400)

This is an example of using a greedy algorithm in graph theory to find a minimum spanning tree for a connected weighted graph. Vertices represent the cities and edges represent the roads to be rebuilt. Red digits indicates the amount of money to rebuild that stretch of road.

To solve the problem, 3 problems have to be addressed.

1. How to represent the graph?
2. How to select the edges in nondecreasing order by its weight?
3. How to determine whether adding an edge would create a cycle?

The algorithm pseudocode for this problem follows the Kruskal algorithm:

	kruskal_algorithm(edgelist, n) {
		sort(edgelist)
		for i = 1 to n
			makeset(i)
		count = 0
		i = 1
		while count < n-1:
			if(findset(vertex1) != findset(vertex2)) {
				count += 1
				union(vertex1, vertex2)
			}
			i += 1
	}

First, we use a 2d array to represent each of the edges in the graph. eg. [1,2] would indicate an edge between city 1 and 2. Then we store the edge's corresponding weight in another array with the weight as its element. eg.[4] for city 1 and 2. We also need a 3rd array to store all the vertices involved(purpose to be discussed). To avoid excessive parameter passing, let's declare a Weighted_graph class:

	class Weighted_graph:
	"""
	Implement a weighted graph with a 2d list and a corresponding weight list
	Vertices are represented by elements(eg. 1,2,3) in the graph list and edges are represented by a list collection of each 2 vertices
	(eg. [1,2])
	"""
	edges = []
	weight = []
	vertices = []

	def __init__(self, edge_list, weight):
		self.edges.append(edge_list)
		self.weight.append(weight)

Given the above graph, the corresponding data would be represented in our class attributes as the following:
	
	edges = [[3, 4], [1, 3], [5, 6], [1, 5], [3, 6], [1, 2], [2, 4], [3, 5], [4, 6]]
	weight = [4,2,3,5,1,6,3,6,2]
	vertices = [[1],[3],[4],[5],[6],[2]]
	
We are also going to add utility functions, add() and print() to aid in our implementation.

	def add(self, edge_list, weight):
		"""
		Add an edge(defined by 2 vertices in a list) and its corresponding weight to edges
		"""
		self.edges.append(edge_list)
		self.weight.append(weight)
	
	def print_graph(self):
		"""
		Print each set of edges in a graph and its corresponding edges
		"""
		print self.edges
		print self.weight
		print self.vertices
Second, in order to select edges in nondecreasing order, we need to sort its corresponding weight in nondecreasing order. Here we use a simple insertion sort algorithm to do the job:
	
	def __sort(self):
		"""
		Sorts both edges and weight lists in nondecreasing order of weight list elements
		"""	
		if len(self.edges) != len(self.weight):
			return
		for i in range(1, len(self.weight)):
			temp_weight = self.weight[i]
			temp_edge = self.edges[i]
			current = i - 1
			while current >= 0 and temp_weight < self.weight[current]:
				self.weight[current+1] = self.weight[current]
				self.edges[current+1] = self.edges[current]
				current -= 1
			self.weight[current+1] = temp_weight
			self.edges[current+1] = temp_edge
			
Next we need to be able to initialize each vertex to its own component in vertices array. Let's use a private makeset function to achieve this:
	
	def __makeset(self):
		"""
		Initialize each vertex to its own component
		"""
		for i in range(len(self.edges)):
			for j in range(len(self.edges[i])):				
				if self.edges[i][j] not in self.vertices:
					self.vertices.append(self.edges[i][j])
		
		for k in range(len(self.vertices)):
			self.vertices[k] = [self.vertices[k]]

Then we need a way to find the corresponding vertex in the vertices list of list and return a index to the location of the vertex in question.
	
	def __findset(self, vertex):
		"""
		Find and return the index to which vertex belongs in vertices list
		"""		
		for i in range(len(self.vertices)):
			for element in self.vertices[i]:
				if element == vertex:
					return i
		return None
		
Just 1 last function! We need to be able to merge the 2 vertex in vertices list together to indicate that their corresponding edge have been selected. We use a union function to manipulate the vertices list directly.
	
	def __union(self, vertex1, vertex2):
		"""
		Joins 2 vertex together 		
		"""
		index1 = self.__findset(vertex1)
		index2 = self.__findset(vertex2)
		for element in self.vertices[index2]:
			self.vertices[index1].append(element)
		self.vertices.pop(index2)
		
So we have all the functions we need to implement Kruskal's algorithm. Let's convert the pseudocode earlier to python codes...
	
	def kruskal(self):
		self.__sort()
		self.__makeset()
		count, i = 0, 0		
		while len(self.vertices) > 1:
			if self.__findset(self.edges[i][0]) != self.__findset(self.edges[i][1]):
				print "(%d %d) edge selected." % (self.edges[i][0], self.edges[i][1])
				count += 1
				self.__union(self.edges[i][0], self.edges[i][1])
			i += 1
			
With this implementation, we test run the codes and should be able to get the following output.
	
	(3 4) edge selected.
	(1 3) edge selected.
	(5 6) edge selected.
	(1 5) edge selected.
	(1 2) edge selected.

Keep in mind that our vertices list starts off like this:

	vertices = [[1],[3],[4],[5],[6],[2]]

As each set of edges is selected, its 2 cities are combined to indicate selection. This helps in determining if the graph is cyclic at each new edge selection stage. Finally, our vertices list looks like this, which indicates all the vertices are connected minimally.	
	
	vertices = [[1, 3, 4, 5, 6, 2]]
	
Kruskal's algorithm is a optimal implementation of greedy algorithm. Here's my [full implementation](https://github.com/hguochen/algorithms/blob/master/python/kruskal.py).
			