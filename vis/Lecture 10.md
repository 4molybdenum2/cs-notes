Information navigation

1. how can we deal with info overload?
2. allow users to control (extent of data, level of detail, aspects of data)
3. provide navigation hints
	1. sort
	2. rescale (scale to some range, better for vis, normalize?)
	3. re-express (suppose x can be written as 1/x)
	4. filtering
	5. zooming
	6. aggregate (Biplots, MDS, PCP)
	7. info space overview + detail
	8. fisheye lens (keeps target item in focus, but causes distortion)
		1. less relevant items are dropped from peripheral
	9. perspective wall
		1. details on center panel are larger
	10. magic lens (shows details using panning on an area)
	11. zoom and pan -> transfer of focus (zoom out -> pan -> zoom in)
		1. panning: smooth movement of viewing frame over 2D image of larger size
		2. zooming: increasing magnification of a decreasing fraction of 2D image under constant viewing frame of constant size
	12. semantic zoom -> shows a different representation depending on available space -> like google maps !
		1. can switch datasets to the one with higher res
		2. synthesize detail on each level of zoom
		3. constrained synthesis
		4. preference function for semantic zoom (zoom based on price, preference, etc)
	13. exploded views
	14. brushing and linking
		1. atleast two things must be linked for this to work -> for example two views


Important rules for dashboard:
1. most important views goes on the top or top left
2. 5 views or less
3. interactivity
4. avoid multiple color schemes