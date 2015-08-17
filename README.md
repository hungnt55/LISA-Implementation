Information:
--------------------------------------------------------
Version 1.0: Implementation of LISA method for Influence Maximization and Influence Spectrum under Independent Cascade(IC) or Linear Threshold(LT) model. For more details about LISA, please read our paper: "T. N. Dinh, H. T. Nguyen, P. Ghosh and M. Mayo, Social Influence Spectrum with Guarantees: Computing More in Less Time, International Conference on Computational Social Networks (CSoNet), 2015"


Terms of use:
--------------------------------------------------------
The LISA software is released under a dual licence.

To give everyone maximum freedom to make use of LISA and derivative works, we make the code open source under the GNU Affero General Public License version 3 or any later version (see LICENSE_AGPLv3.txt.) We are not responsible for any demage caused during the use of our code.


Requirements:
--------------------------------------------------------
In order to compile all the tools, it requires GCC 4.7.2 and later (which also support OpenMP 3.1).


Compile:
--------------------------------------------------------
Use `make' command to compile everything


How to use:
--------------------------------------------------------
This package offers a set of functions to use in order to find seed sets with the number of seeds ranging from kl to ku. A typical sequence of actions is:

1. Conversion from a text format to binary file
	./el2bin <input file> <output file>

    <input file>: the path to text file in edge list format: the first line contains the number of nodes n and number of edges m, each of the next m lines describes an edge following the format: <src> <dest> <weight>. Node index starts from 1.
    <output file>: the path to binary output file

2. Run LISA to find the seed sets

	./LISA [Options]

    Options:

        -i <binary graph file>
            specify the path to the binary graph file (default: network.bin)
        -o <seed output file>
            specify the path to the output file containing selected seeds (default: seeds.out)
        -kl <number of seeds>
            number of selected seed nodes in the first seed set(default: 1)
        -ku <number of seeds>
            number of selected seed nodes in the last seed set(default: n)
        -e <epsilon>
            epsilon value in (epsilon,delta)-approximation (see our paper for more details, default: 0.1)
        -d <delta>
            delta value in (epsilon,delta)-approximation (default: 0.01)
        -t <number of threads>
            number of running threads (default: 1)
        -m <model>
            diffusion model (LT or IC, default: LT)

     Output format:
	The outputs are printed on standard output stream in the following order
	
		First seed set: S_1, S_2,..., S_kl
		Influence: [Influence of the first kl-seed set], Time: [Total time taken], Memory: [Memory used]
		Other seed sets:
		[Addition seed for (kl+1)-seed set], [Influence of (kl+1)-seed set]
		[Addition seed for (kl+2)-seed set], [Influence of (kl+2)-seed set]
		...
		[Addition seed for ku-seed set], [Influence of ku-seed set]

3. (Optional) Verify influence spread of a seed set
	./verifyInf <binary graph file> <seed file> <epsilon> <number of threads> <model: LT or IC>


Examples: find seed sets with number of seeds ranging from 1 to n on the graph network.txt:
The sample network network.txt in this case contains only 4 nodes and 4 edges and is formated as follows:

		4 4
		1 2 0.3
		2 3 0.5
		1 3 0.6
		1 4 0.2
		
	1. Convert to binary file:
	
		./el2bin network.txt network.bin
	
	2. Run LISA with kl=1 and ku=4, epsilon=0.1,delta=0.01:
	
		./LISA -i network.bin -kl 1 -ku 4 -e 0.1 -d 0.01
	
	The output after running LISA:

		First seed set: 1
		Influence: 2.04, Time: 0s, Memory: 17.6328 MB
		Other seed sets: 
		2, 3.08
		4, 3.89
		3, 4.00

	Here, the seed sets with their influence spread are:1-seed set {1} with the influence 2.04, 2-seed set: {1,2} with 3.08, 3-seed set: {1,2,4} with 3.89, 4-seed set: {1,2,4,3} with 4.00. The running time is 0s and memory used is 17.6328 MB.
