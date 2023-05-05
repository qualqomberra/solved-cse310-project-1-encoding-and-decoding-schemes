Download Link: https://assignmentchef.com/product/solved-cse310-project-1-encoding-and-decoding-schemes
<br>
Encoding and decoding schemes are used in a wide variety of applications, such as in music or video streaming, data communications, storage systems (<em>e.g.</em>, on CDs, DVDs, RAID arrays), among many others. In a <em>fixedlength encoding </em>scheme each character is assigned a bit string of the same length. An example is the standard <a href="http://www.asciitable.com/">ASCII code</a><a href="http://www.asciitable.com/">.</a> One way of getting an encoding scheme that yields a shorter bit string on the average is to assign shorter codewords to more frequent characters and longer ones to less frequent characters. Such a <em>variable-length encoding </em>scheme was used in the telegraph code invented by Samuel Morse. In that code, frequent letters such a e (·) and a (· -) are assigned short sequences of dots and dashes while infrequent letters such as q (- – · -) and z (- – ··) have longer ones.

In this project you will implement a variable-length encoding and decoding scheme, and run experiments to evaluate the effectiveness of the scheme, and the efficiency of your algorithms.

<strong>Note: </strong>This project <strong>must </strong>be completed <strong>individually</strong>. Your implementation <strong>must </strong>use C/C++ and ultimately your code <strong>must </strong>run on the Linux machine general.asu.edu.

You <strong>may not </strong>use any external libraries to implement any part of this project, aside from the standard libraries for I/O and string functions (stdio.h, stdlib.h, string.h, and their equivalents in C++). If you are in doubt about what you may use, ask me.

By convention, your program should exit with a return value of zero to signal that all is well; various non-zero values signal abnormal situations.

You <strong>must </strong>use a version control system as you develop your solution to this project, <em>e.g.</em>, GitHub or similar. Your code repository <strong>must </strong>be <em>private </em>to prevent anyone from plagiarizing your work.

The rest of this project description is organized as follows. §1 gives the requirements for Project #1 including a description of the encoding and decoding schemes to implement in §1.1 and §1.2, respectively. §2 gives the experiments to design to evaluate the effectiveness and efficiency of the encoding and decoding schemes. Finally, §3 describes the submission requirements for the milestone and full project deadlines.

<h1>1           Program Requirements for Project #1</h1>

<ol>

 <li>Write a C/C++ program that implements the encoding scheme described in §1.1 on plain text. The program <strong>must </strong>be compiled into an executable named encode and it <strong>must </strong>take one command line parameter. The parameter is one of the keywords insertion or quick. In all cases, you <strong>must </strong>read input from stdin, allowing redirection from a text file. You <strong>must </strong>write the encoded plain text to stdout, allowing redirection to a text file. See §1.1 for detailed instructions.</li>

 <li>Write a C/C++ program that implements the decoding scheme described in §1.2 on input in the format produced by encoding scheme as prescribed in §1.1. The program <strong>must </strong>be compiled into an executable named decode. In all cases, you <strong>must </strong>read input from stdin allowing redirection from an encoded text file. You <strong>must </strong>write the decoded input to stdout, allowing redirection to a text file. The decoded input should match the original input, <em>e.</em>, the input prior to encoding. See §1.2 for detailed instructions.</li>

 <li>Design experiments to evaluate your programs as described in §2. A brief report with figures plotting the data you collect, and interpretation of your results is expected.</li>

</ol>

Sample text input will be provided on Canvas; use them to test the correctness of your programs. Scripts will be used to check the correctness of your program. Therefore, <strong>absolutely no changes to these project requirements are permitted</strong>.

<h2>1.1         The Encoding Algorithm</h2>

Given a normal text file (redirected from stdin), for each line of the file:

<ol>

 <li>Transform the line into a form that is more amenable to compression. The transformation rearranges the characters in the input into many clusters of repeated characters, in a way that it is possible to recover the original input.</li>

 <li>Output the compressed form of the transformed line.</li>

</ol>

To transform a line of text, treat the line as a string of length <em>N</em>. First, compute (and store) every cyclic shift of the string, shifting to the left by one character. Then sort the <em>N </em>cyclic shifts in lexicographic order according to the ASCII code. Note the ordering of characters in the <a href="http://www.asciitable.com/">ASCII code</a><a href="http://www.asciitable.com/">.</a>

Table 1 shows the transformation step for the example string Mississippi, of length <em>N </em>= 11. Under <strong>Original</strong>, rows with index 0<em>,…,</em>10 show the cyclic shift of the string to the left by as many characters, <em>e.g.</em>, at index 5 is the string shifted cyclically to the left by 5 characters. Under <strong>Sorted</strong>, all <em>N </em>cyclic shifts of the string are sorted in lexicographic order. (The next column is used in decoding, so it is discussed in §1.2.)

Table 1: Transformation Step of the Encoding Algorithm

<table width="476">

 <tbody>

  <tr>

   <td width="54"><strong>Index</strong></td>

   <td width="162"><strong>Original</strong></td>

   <td width="54"><strong>Index</strong></td>

   <td width="162"><strong>Sorted</strong></td>

   <td width="44">next</td>

  </tr>

  <tr>

   <td width="54">0</td>

   <td width="162">M i s s i s s i p p i</td>

   <td width="54">0</td>

   <td width="162">M i s s i s s i p p i</td>

   <td width="44">4</td>

  </tr>

  <tr>

   <td width="54">1</td>

   <td width="162">i s s i s s i p p i M</td>

   <td width="54">1</td>

   <td width="162">i M i s s i s s i p p</td>

   <td width="44">0</td>

  </tr>

  <tr>

   <td width="54">2</td>

   <td width="162">s s i s s i p p i M i</td>

   <td width="54">2</td>

   <td width="162">i p p i M i s s i s s</td>

   <td width="44">6</td>

  </tr>

  <tr>

   <td width="54">3</td>

   <td width="162">s i s s i p p i M i s</td>

   <td width="54">3</td>

   <td width="162">i s s i p p i M i s s</td>

   <td width="44">9</td>

  </tr>

  <tr>

   <td width="54">4</td>

   <td width="162">i s s i p p i M i s s</td>

   <td width="54">4</td>

   <td width="162">i s s i s s i p p i M</td>

   <td width="44">10</td>

  </tr>

  <tr>

   <td width="54">5</td>

   <td width="162">s s i p p i M i s s i</td>

   <td width="54">5</td>

   <td width="162">p i M i s s i s s i p</td>

   <td width="44">1</td>

  </tr>

  <tr>

   <td width="54">6</td>

   <td width="162">s i p p i M i s s i s</td>

   <td width="54">6</td>

   <td width="162">p p i M i s s i s s i</td>

   <td width="44">5</td>

  </tr>

  <tr>

   <td width="54">7</td>

   <td width="162">i p p i M i s s i s s</td>

   <td width="54">7</td>

   <td width="162">s i p p i M i s s i s</td>

   <td width="44">2</td>

  </tr>

  <tr>

   <td width="54">8</td>

   <td width="162">p p i M i s s i s s i</td>

   <td width="54">8</td>

   <td width="162">s i s s i p p i M i s</td>

   <td width="44">3</td>

  </tr>

  <tr>

   <td width="54">9</td>

   <td width="162">p i M i s s i s s i p</td>

   <td width="54">9</td>

   <td width="162">s s i p p i M i s s i</td>

   <td width="44">7</td>

  </tr>

  <tr>

   <td width="54">10</td>

   <td width="162">i M i s s i s s i p p</td>

   <td width="54">10</td>

   <td width="162">s s i s s i p p i M i</td>

   <td width="44">8</td>

  </tr>

 </tbody>

</table>

The compressed output for a non-empty line consists of two lines:

<ol>

 <li>The index of the row in which the original string appears in the <strong>Sorted </strong></li>

 <li>Form a string last consisting of the last character of each <strong>Sorted </strong> In this string, which is some permutation of the original string, characters form clusters of characters of size one or more. To encode last, step through the string from left to right processing the clusters: For each cluster, output the cluster size, followed by a blank character, followed by the character in the cluster.</li>

</ol>

For the example string, the original string appears at index position zero in the <strong>Sorted </strong>column. The last character of each <strong>Sorted </strong>string is a new string last=ipssMpissii. The first two characters are each in a cluster of size one. This is followed by a cluster of size two of the character s, and so on. Therefore, the encoding of the string ipssMpissii is:

0

1 i 1 p 2 s 1 M 1 p 1 i 2 s 2 i

<h3>1.1.1         Input to the Encoding Algorithm</h3>

The encoding algorithm has one input parameter taken from the command line: It is a keyword indicating the sorting algorithm to use. The text to encode <strong>must be </strong>read from stdin, which may be redirected from a file in Unix/Linux format. It is important to know that in Window files, the end of line is signified by two characters, Carriage Return (CR) followed by Line Feed (LF) while in Linux, only LF is used. Similarly, your output <strong>must be </strong>written to stdout, which may be redirected to a text file.

Some examples of input to your encode program:

encode insertion &lt;ex1.txt &gt;encoded_ex1.txt encode quick &lt;ex2.txt &gt;encoded_ex2.txt

You must implement both the Insertion Sort algorithm and the Quicksort sorting algorithms. If the keyword is equal to insertion then you must use Insertion Sort to sort the strings in <strong>Original</strong>. Likewise, if the keyword is quick then the sorting algorithm used is Quicksort.

<strong>Do not sort the strings directly because it will result in too much data movement. </strong>To be efficient, sort pointers to the strings instead.

<h3>1.1.2         Output of the Encoding Algorithm</h3>

The output of your encoding scheme is text in which each line of the input is compressed as described in §1.1. Two lines of output are produced for every non-empty line of input. <em>If a line consists of a LF control character only, then the line is empty; the output is a LF character.</em>

<h2>1.2         The Decoding Algorithm</h2>

Now we describe how to decode a line, <em>i.e.</em>, recover the original line. There are three steps in the recovery:

<ol>

 <li>Read the integer giving the index of the row in which the original string appears in the <strong>Sorted </strong></li>

 <li>Recover the string last.</li>

 <li>Using the index, last, and knowledge of the next column, recover the original string.</li>

</ol>

Recall that the encoded output of the line with the string Mississippi is:

0

1 i 1 p 2 s 1 M 1 p 1 i 2 s 2 i

In this case, the index is zero. Recovering the string last=ipssMpissii from the encoded string is straightforward. Using index, last, and knowing the next column in Table 1, makes decoding easy, as given by the following pseudocode (which, of course, you must generalize):

int i, index, pos; index = 0; // Index of row in which original string appears int next[11] = { 4, 0, 6, 9, 10, 1, 5, 2, 3, 7, 8 }; // See section 1.2.1 char last[12] = “ipssMpissii”; // Recovered string last pos = next[ index ]; for( i = 0; i &lt; 11; i++ ){ putchar( last[ pos ] ); pos = next[ pos ];

}

This results in the output Mississippi, <em>i.e.</em>, the original string of the line is successfully recovered.

<h3>1.2.1         Computing the next Column</h3>

We now describe how the next column is computed from the <strong>Sorted </strong>strings. For <em>i </em>= 0<em>,…N </em>− 1, next[<em>i</em>] is the index of the row containing the cyclic shift of <strong>Sorted[</strong><em>i</em><strong>] </strong>to left by one character. For the example in Table 1, <strong>Sorted[0] </strong>is Mississippi. Shifting this string to left by one character gives ississippiM, and this string can be found at <strong>Sorted[4]</strong>. Hence next[0] is 4. <strong>Sorted[1] </strong>is iMississipp. Shifting this string to left by one character gives Mississippi, and this string can be found at <strong>Sorted[0]</strong>. Hence next[1] is 0. <strong>Sorted[2] </strong>is ippiMississ. Shifting this string to left by one character gives ppiMississi, and this string can be found at <strong>Sorted[6]</strong>. Hence next[2] is 6. Continuing in this way for <em>i </em>= 3<em>…</em>10 gives the values in the column next in the table. Indeed, next is just a permutation of the indices 0<em>,…,N </em>− 1.

What is amazing is that the information in the encoding is enough to reconstruct next, and therefore the original message! From last, we know all of the characters in the original string, they’re just permuted. We can reconstruct the first column in <strong>Sorted </strong>by sorting the characters in last; see Table 2.

Because M only occurs once in the string and the array is formed using cyclic shifts, we can deduce that next[0] = 4 because M is in the last column of row with index 4. However all the other characters are in clusters of size larger than one, so how can we tell how to compute next? For character p, it may seem ambiguous whether next[5] = 1 and next[6] = 5, or whether next[5] = 5 and next[6] = 1.

Table 2: Reconstructing next from the Encoding

<table width="323">

 <tbody>

  <tr>

   <td width="54"><strong>Index</strong></td>

   <td width="225"><strong>Sorted</strong></td>

   <td width="44">next</td>

  </tr>

  <tr>

   <td width="54">0</td>

   <td width="225">M ? ? ? ? ? ? ? ? ? i</td>

   <td width="44">4</td>

  </tr>

  <tr>

   <td width="54">1</td>

   <td width="225">i ? ? ? ? ? ? ? ? ? p</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">2</td>

   <td width="225">i ? ? ? ? ? ? ? ? ? s</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">3</td>

   <td width="225">i ? ? ? ? ? ? ? ? ? s</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">4</td>

   <td width="225">i ? ? ? ? ? ? ? ? ? M</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">5</td>

   <td width="225">p ? ? ? ? ? ? ? ? ? p</td>

   <td width="44">1</td>

  </tr>

  <tr>

   <td width="54">6</td>

   <td width="225">p ? ? ? ? ? ? ? ? ? i</td>

   <td width="44">5</td>

  </tr>

  <tr>

   <td width="54">7</td>

   <td width="225">s ? ? ? ? ? ? ? ? ? s</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">8</td>

   <td width="225">s ? ? ? ? ? ? ? ? ? s</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">9</td>

   <td width="225">s ? ? ? ? ? ? ? ? ? i</td>

   <td width="44"> </td>

  </tr>

  <tr>

   <td width="54">10</td>

   <td width="225">s ? ? ? ? ? ? ? ? ? i</td>

   <td width="44"> </td>

  </tr>

 </tbody>

</table>

As it turns out, there is a rule that resolves the ambiguity. It is: If row index <em>i </em>and <em>j </em>both start with the same letter and <em>i &lt; j</em>, then next[<em>i</em>] <em>&lt; </em>next[<em>j</em>]. This rule implies that next[5] = 1 and next[6] = 5.

Why is this rule valid? The rows are sorted, so row 5 is lexicographically less than row 6. This means that the nine unknown characters in row 5 must be less than the nine unknown characters in row 6 (since both rows start with the letter p). We also know that between the two rows that end with p, row 1 is less than row 5. But, the nine unknown characters in row 5 and 6 are precisely the first nine characters in rows 1 and 5. Thus, next[5] = 1 and next[6] = 5 or this would contradict the fact that the strings are sorted.

Using the rule allows all remaining ambiguities to be resolved and all entries of next to be computed.

<h3>1.2.2         Input to the Decoding Algorithm</h3>

The input to the decoding scheme is text in the form generated by the encoding scheme. As with the encoding scheme, the text to decode <strong>must </strong>be read from stdin, which may be redirected from a file in Linux format. Similarly, your output <strong>must </strong>be written to stdout, which can be redirected to a text file.

If the input to the encoding scheme consists of <em>k </em>non-empty lines, then its output has 2<em>k </em>encoded lines. Thus the decoding scheme iterates <em>k </em>times in order to decode the <em>k </em>encoded non-empty lines of input.

<h3>1.2.3         Output of the Decoding Algorithm</h3>

The output of the decoding scheme should equal the input to the encoding scheme. Note that some care will be needed to take care of the LF characters so that the lines match. (Try using the diff commands to compare the two files.)

<h1>2           Experimentation</h1>

A standard measure of the “goodness” of a compression algorithm’s effectiveness is the <em>compression ratio</em>. This is the ratio 100%, where <em>t </em>is the total number of characters in the input, and <em>c </em>is the number of clusters in the encoding.

For example, the encoding of Mississippi is 1 i 1 p 2 s 1 M 1 p 1 i 2 s 2 i. In this example, the total number of characters is <em>t </em>= 11, the number of clusters is <em>c </em>= 8, and so the compression ratio is or 27%. (Here, we are ignoring the fact we used integers to code the cluster sizes; this can be done more intelligently but this project is already enough work, right? ,) Design a set of experiments to study:

<ol>

 <li>The average compression ratio; in addition to the average, compute the minimum, maximum, and standard deviation of the compression ratio. You might consider using a box and whiskers plot for this metric.</li>

 <li>The time to encode each input for each type of sort, <em>e.</em>, for Insertion and for Quicksort. Plot the run time as a function of input size.</li>

 <li>The time to decode each encoded input. Plot the run time as a function of input size.</li>

 <li>The compression ratio as a function of number of lines encoded. The encoding algorithm has been described as encoding one line at a time. If you instead encode 2<em>,</em>3<em>,… </em>lines at a time, does the compression ratio improve? What do you expect to happen?</li>

</ol>