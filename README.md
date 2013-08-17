
<html><head><title>CAS CS 440/640 Artificial Intelligence - Spring 2012</title></head>

<body bgcolor="white">

<center>
<a href="http://www.bu.edu/"><img src="http://www.cs.bu.edu/fac/betke/images/bu-logo.gif" border="0" height="120" width="119"></a>

<h2> CAS CS 440/640  Artificial Intelligence - Spring 2012  </h2>

<h2>Programming Assignment 3 </h2>
<h2> Hidden Markov Models and Natural Language Processing </h2>

</center>




     <b>Overview of Tasks</b> </p>

<p>This project will require you to build a basic English sentence
recognizer based on hidden Markov models ("HMMs").  An HMM was defined
for you in advance based on a simplified version of English.  This
version has only very limited vocabulary and does not use articles or
prepositions. Only the following words are included in the vocabulary
supported by the HMM: <br><br> {"kids", "robots", "do", "can", "play",
"eat", "chess", "food"}<br><br> The HMM is expected to recognize
and parse sentences that use the above vocabulary. The
sentences<br><br> "kids play chess" <br> "can robots eat
food"<br><br> are two examples of "good" English sentences. The
sentences <br><br> "play food kids"<br><br> is considered a "bad"
English sentence, <br>

<p>
You are required to implemente this HMM so that it handles various
recognition tasks. Specifically, given the Hidden Markov model
and some input sentences, you are asked to write programs that perform
the following: </p> <ol> <li><b>Pattern Recognition</b>:
Implement the "forward part" of the forward/backward procedure
discussed in class. (The procedure is also described in
<i>A Tutorial on Hidden Markov Models and Selected Applications in
Speech Recognition</i> by Rabiner [1989].) &nbsp; Given the HMM and
several observation sequences, <tt>recognize</tt> should report the
observation probability of each input sequence.  </li> <br> &nbsp;
<li><b>State-Path Determination</b>: Implement the Viterbi algorithm
to determine the optimal state path for each observation set and
report its probability.  &nbsp; </li> <br> &nbsp; <li><b>Model
Optimization</b>: Optimize the HMM using the Baum-Welch algorithm and
report the probabilities before and after optimization.</li>
</ol>


<p>     <b>Additional Information</b> </p>

<ol>
  <li> You are only required to implement re-estimation formulas
    (Baum-Welch Algorithm) defined in Eqs. 40a, 40b, 40c (Rabiner).
    Given an initial set of HMM parameters (sentence.hmm) and a file
    containing possibly multiple observation sequences, your system
    will have to produce as many HMMs as there are input
    sequences. Each of these HMMs will be the result of running just
    one iteration of the Baum-Welch Algorithm on the corresponding
    input sequence. You should report a pair of numbers: the
    probability of the observation sequence given the original HMM and
    the re-estimated HMM. Your system will output as many pairs as
    there are observation sequences in the input file.

    <p>
For extra credit, you may choose to handle multiple observation
sequences (Eqs. 109, 110). This is a more challenging but rewarding
case, as you will produce just one HMM whose parameters are updated
based on all of the sequences. For left-to-right HMMs, training with
multiple observation sequences is very important (see bottom of p. 17
of Rabiner's paper), but for the purpose of this short programming
project, we let you skip it.
</p>

  <li> You can use C/C++ or Javato implement this assignment. However,
    you still need to implement all the programs with the command-line
    switches defined below.  </ol>
<p>


<p><b>File Format to Describe a HMM</b></p>

<p>The format of an <tt>.hmm</tt> file is as follows:&nbsp; <br>
The first line contains integers <i>N</i> (number of states),
   <i>M</i> (number of observation symbols), and <i>T</i> (number of time
steps or length of oberservation sequences).&nbsp;  <br>
The second contains four strings <br>
<i>SUBJECT AUXILIARY PREDICATE OBJECT</i> ,<br> which refer to four
basic English syntactic structures. Each is used to name an individual
HMM state.<br> The third line contains strings<br>
<i>kids robots do can play eat chess food,</i> <br> that provide the
vocabulary to be used in the observation sentences. <br> Then comes a
line with the text "a:", followed by the matrix <i>a</i>. &nbsp; The
matrix <i>b</i> and vector <i>pi</i> are similarly represented.&nbsp;
The matrix and vector elements are floating-point numbers less than or
equal to 1.0. </p>

Please note that the HMM should be in the correct form, which means
the percentages across rows add up to 1.0.  Moreover, the HMM and a
set of observation data use the same finite alphabet of observation
symbols.  For purposes of this assignment, you may assume that any
state can be a final state.  

<p>Here is the provided <a
href="sentence.hmm"><tt>.hmm</tt> </a>file. <br>
</p>
<p> <b>File Format to Describe an Observation Sequence</b></p>

<p>The format of a <tt>.obs</tt> file is as follows:&nbsp; The first line
   of the file contains the number of data sets in the file.&nbsp; For each
  data set, the number of observations in the set appears on a line by itself;
  on the next line are the observations, composed by the tokens from the vocabulary. &nbsp;
  All observation tokens in this programming assignment map to integers stored as the observation sequence. </p>

<p>Here are two sample files: <a href="example1.obs"><tt>example1.obs</tt></a> and <a href="example2.obs"><tt>example2.obs</tt></a>.
   </p>

<p><b>Program Interface</b></p>

   Your programs will take as command-line arguments the name of the
  file containing the HMM and the name of the file containing the
  observation sets.&nbsp; For example, to read the HMM from
  <tt>sentence.hmm</tt> and observations from <tt>example1.obs:</tt>
  <blockquote><tt>./recognize sentence.hmm example1.obs</tt> <br>
  <tt>./statepath sentence.hmm example1.obs</tt></blockquote>

<p><tt>optimize</tt> also takes the output file as
  a command-line argument. To read the HMM from <tt>example2.hmm</tt>
  and observations from 
<tt>example2.obs</tt>,
   and save the optimized version of the HMM in <tt>sentence-optimized.hmm</tt>: </p>

<blockquote><tt>./optimize sentence.hmm example2.obs sentence-optimized.hmm</tt></blockquote>


<p><b>Required Program Output</b></p>

<p>Your programs will produce output in the following format:&nbsp;
<br>
   There is one line of output for each data set.&nbsp;
   </p><p>

   <tt>recognize</tt>:

   </p><ul>
   Each line reports P(O | lambda) for that HMM,
   <i>e.g.,</i> <p></p>

<blockquote><tt> TeachingCS440 % ./recognize sentence.hmm example1.obs <br>
0.027 <br>
0.0288 <br>
0.0 </tt></blockquote>
      indicates that HMM will observe the first sequence
 with probability 2.7%, and the  second seqeunce with probability 2.88%, respectively.<br>
<i><b>Question:</b></i>&nbsp; For the current application, why does
this probability seem lower than we expect? What does this probability
tell you?  Does the current HMM always give a reasonable answer? For
instance, what is the output probability for the below
sentence?<br><br> "robots do kids play chess"<br> "chess eat play
kids"<br><br> You may want to use different observation data to
justify your answer.  </ul>

<tt>statepath</tt>:

<ul>Each line again corresponds to a data
set:&nbsp; In this case, the program outputs
the "optimal" state path<i>, i.e.,</i> the path with the highest probability
P(O, I | lambda) and, if the probability is greater than zero,
the state sequence.&nbsp; For example, <p></p>

<blockquote><tt>TeachingCS440 % statepath sentence.hmm example1.obs <br>
0.027 SUBJECT PREDICATE OBJECT<br>
0.0288 SUBJECT PREDICATE OBJECT<br>
0 </tt> <br> </blockquote>
      indicates that the probability of the first optimal path is 2.7%, and the
tokens associated with the optimal state sequence is <i>"SUBJECT", "PREDICATE", "OBJECT"</i>. &nbsp; For the third input, since the probability
of the obvervation is 0.0, no optimal path can be computed.<br>
<i><b>Question:</b></i>&nbsp; What can we tell from the reported optimal path for syntax analysis purpose? Can the HMM always correctly distinguish
"statement" from "question" sentence? Why?
</ul>

<tt>optimize</tt>:
<ul>
This program optimizes the HMM using one iteration
  of the Baum-Welch algorithm.&nbsp; After all data sets are processed, <tt>optimize</tt>
   saves the optimized HMMs in a new file as specified by the command-line
 argument.  <p></p>

<p>For each data set, <tt>optimize</tt> prints out P(O
   | lambda) for the old HMM before optimization, and P(O | lambda) for the new HMM after optimization:
   </p>

<blockquote><tt>TeachingCS440 % optimize sentence.hmm example2.obs sentence-opti.hmm <br>
0.000588 0.037037</tt> <br> </blockquote>
      indicates that, for the first data set, HMM observes the highest probability
   (0.0588%) and, after optimization, it has probability 3.7037%.&nbsp; The resulting optimized HMM can be found <a href="sentence-opti.hmm">here</a>. <br>
 <i><b>Question:</b></i>&nbsp;
Why should you not try to optimize an HMM with zero observation probability?

</ul>

<p>Do not include extraneous output, such as debugging information, in your
  final programs. <br>
     &nbsp; </p>


<p><b>Model Enhancement</b></p>

<p>
Now supposed you want this HMM to model new syntax structures, like
"PRESENT TENSE" or "ADVERB," so that the following sentences can be
parsed:
<br>


"robots can play chess well"<br>
"kids do eat food fast"<br><br>
  <i><b>Question:</b></i>&nbsp; What kinds of changes will you need to make in 
  the above HMM? Please describe your solution with an example of the modified 
  matrices <i>a, b</i> and <i>pi</i> in the submitted web page.</p>

<p><b>Submission</b> </p>

<p>Zip/tar your source code, project files, test data and report into a single 
  archive and gsubmit.</p>


<p><b>Grading Criteria</b> </p>

<p><i>Please note that you will not get any credit for programs that do not build 
  properly. </i></p>

<p>The grader will test your program with different data sets designed to exploit 
  any weaknesses in your code.&nbsp; Your code should handle any legal observation 
  sets.&nbsp; </p>
<ul>
  <li>10 points for a program that properly reads input and writes output in required 
    format. </li>
  <li>20 points for a program that properly implements the "forward" part of the 
    "forward/backward" algorithm. </li>
  <li>20 points for a program that properly implements the Viterbi algorithm. 
  </li>
  <li>20 points for a program that properly implements Baum-Welch. </li>
  <li>30 points for a description of what you accomplished and correct answers 
    to the questions. Describe in the web page file how well your programs work. 
    Do you believe your solution to be correct? If not, describe what you were 
    successful in completing, and where you had problems.<br>
  </li>
</ul>
<p><b>Team Work and Academic Conduct Reminder</b> </p>

<p>We encourage team work. If you work in a team, all team members
  need to contribute to the solutions. Submitting code found on the
  internet will be considered an act of plagiarism. Submissions will
  be checked for originality by hand and by a program checker - please
  do not jeopardize your academic standing by taking chances in
  completing this assignment. Please come see Prof. Betke or Zhiqiang
  (Alex) if you have any questions about where to draw the line
  regarding acceptable levels of collaboration.<br>


  </p>
     <br>
     <br>
      <br>
     <br>
  <br>
 <br>

 <p>


  
<p></p></body></html>
