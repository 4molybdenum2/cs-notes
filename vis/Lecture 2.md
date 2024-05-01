Data types in Vis:
1. Categorical (describes quality - what type) -> can be ordinal (ordered, ranked - clothing size, academic grades, etc) or nominal (not organized in a logical sequence -> gender, business type, eye color, etc)
2. Numerical (describes quantity - how many) -> (can be continuos, or discrete)
3. Text
4. Time series
5. Graphs and networks
6. Hierarchies


- Images, videos, text, web logs -> feature analysis -> numbers
- keep reference to original data so we can return


Sensor data:
1. motif discovery
2. fourier, wavelet transform

image data:
- value histograms
- gradient histograms
- FFT
- SIFT
- Bag of Features (BoF)

BoF:
study from slides



Text:
- remove stop words and stem data
- perform named-entity recognition
- text to numerical - create term document matrix
	- use LSA to derive visualization
- word embedding - train shallow neural network on a corpus of text 
	- nn vectors represent word similarity as a high dimensional vector
	- use 2d embedding technique to display
- wordcloud


Hierarchies:
1. tree with quantities: how large is each group of stakeholders
2. partition of unity: what fraction is each group with respect to the entire group
3. information flow: how info is disseminated among stakeholders
4. force directed layout: how close are individual stakeholders in term of metric


- solution 1: Numerical to categorical - divide numerical values into equiwidth ranges
- depending on bin size info can be lost
- solution 2: numerical to categorical - divide numerical values into equidepth ranges
- extra storage needed

entropy based binning:
- find bin size with maximal info gain
- Entropy (E) = - sum (p log2 p)
- check slides for info gain calculation


- solution 3: discretization
- what if all bars have seemingly the same height or dominated by one peak? - change to log scaling for y-value



Anti-aliasing:
1. either sample at higher rate or smooth before sampling it - later is called filtering
2. smoothing - stop at each discrete sample point, take avg of window  and store this avg value at that point, move window to next sample point, repeat
3. box filter, gaussian filter
4. detail can;t be refined on zoom, just blurred or replicated, solution? vector graphics - represent detail as a function that can be mathematically refined

**Q. difference between bar charts and histograms?** - from slide 69

how many bars are good?
- 12 if individual categories are a focus
- if overall trend is important - 50 or even more
- eventually you can switch to line plot
- sort bars by height or other metric to aggregate
- find groupings that can aggregate - like countries - continent





