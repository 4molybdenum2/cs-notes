

Hierarchies:
1. Node-edge layout for a hierarchical network
2. color ->  quantitative info

Dendrogram:
1. specifically used to classify classification hierarchies
2. split off points visualize proximity
3. circles are more space efficient to represent classification hierarchies



Chord diagrams:
1. represents flows / connections between several entities
2. make it easier to read using edge bundling
3. hierarchical chord diagrams!
4. following hierarchical edge bundling line is better than straight line
5. levels of bundling:
	1. edges represented by splines with tension B:
		1. setting B: low = low level info, high = high level info
		2. curved edges as spline - Bezier curves, interpolation, BSpline
6. One way to reduce clutter is to replace polylines with poly curves
7. Alpha blending: short curves - more opaque - enables vis of sub bundles and diff of lines
8. Sankey diagram: flow diagram - width is proportional to flow rate:
	1. use cases: where money came from and went to
	2. flow of energy
	3. flow of goods
	4. supply demand graphs
9. Sunburst
10. Treemaps - squarified, slice, slice and dice, strip
	1. cushioned tree maps: textured squares
		1. due to discontuinity in texture, lines are not required anymore to separate
		2. more space can be used for displaying nodes
		3. much smaller nodes can be shown on treemap
11. Voronoi treemaps - based on Voronoi tessellation

