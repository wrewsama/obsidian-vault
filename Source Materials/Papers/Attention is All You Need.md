Tags:
- [[AI ML]]
---

- self attention: relates different positions in the same sequence and outputs a representation of that sequence
- encoder-decoder structure: 
	- encoder maps input sequence $x_1, ... x_n$ to a sequence of continuous representations $z_1, ... z_n$ 
	- decoder generates output sequence $y_1, ... y_m$ one element at a time
	- auto-regressive: model consumes previously generated symbols as additional input
- attention: maps query and set of key-value pairs (expressed as Q, K, and V matrices) to an output
	- more intuitively: the output is a weighted sum of each value, where the weight is computed by a compatibility function of the query and the key corresponding to that value
- multi-head attention:
	- projects Q, K, and V to different dimensions, then does the attention function in parallel
	- allows model to handle info from different relationships
- positional encoding
	- encode the positions within the input embeddings
	- because transformers don't use recurrence / convolution
