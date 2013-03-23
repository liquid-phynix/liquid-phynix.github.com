---
layout: post
title: "Search for SLP"
description: ""
category: 
tags: []
---
{% include JB/setup %}


I recently came across *Qi Cheng*'s paper *On the Ultimate Complexity of Factorials* (you can find it among his [publications](http://www.cs.ou.edu/~qcheng/)) where he describes a Monte Carlo algorithm that computes a straight line program for each input <span>\( n \)</span> that will output a non-zero multiple of <span>\( n! \)</span>. This made me want to write a search program that would find a SLP for a given <span>\( n! \)</span>, that is, it made me interested in the inverse problem. Also, I would like to find such a program of minimum length, for each <span>\( n! \)</span>.

## What is a straight line program?
Straight line programs (SLP) are defined over the integers and the binary arithmetic operators except division. In other words, they are basically nothing else, than a list of operators <span>\( O_n \)</span> and intermediate integer results <span>\( l_n \)</span>. The operators can be addition, subtraction and multiplication. The <span>\( n^{th} \)</span> intermediate result of an SLP is <span>\( l_n = O_n(l_m, l_k) \)</span> where <span>\( m,k \leq n \)</span>. It can be easily seen that we dont lose generality if we fix one of the operands to the previous result of the SLP, i.e. <span>\( l_n = O_n(l_{n-1}, l_m) \)</span>. All SLP's start from the number <span>\( 1 \)</span>, and they have to reach the desired number in <span>\( N \)</span> steps. That is, <span>\( N \)</span> is the length of the SLP.

## The search
The immediate solution to this problem is a brute force Depth First Search (DFS). We fix the length of the SLP, and choose a list of operators, and choose the index <span>\( m \)</span> for each operator (since the other index is the one before it), and evaluate the SLP. We check if it equals to the number we seek, i.e. <span>\( n! \)</span>.

## The program
Below you will find a C++ program that searches through SLP's of length 14 looking for <span>\( n! \)</span> where <span>\( n \)</span> is provided as a command line argument.

{% highlight c++ linenos %}
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
#include <cstdio>

#include <boost/mpi.hpp>
#include <boost/serialization/vector.hpp>

typedef __int128 LI;
typedef LI(*Binop)(LI, LI);

const int RAW = 14;
LI TARGET;
const int OC = 2; //3;

using namespace std;
using namespace boost::mpi;

Binop opers[OC];
const char* annots[OC] = {" (*) ", " (+) "}; //, " (-) "};

LI Op(LI x, LI y){ return x + y; }
LI Ot(LI x, LI y){ return x * y; }
LI Om(LI x, LI y){ return x - y; }

LI fact(LI n){ return (n == 1LL) ? 1 : n * fact(n - 1); }

int OAR[RAW - 1];
int OAR2[RAW - 1];
LI SLP[RAW + 1];

bool checkOperList(){
  int a, b, c, d, e, f, g, h, i, j, k, l, m;
  for(a = 0; a <= 1;  a++){
  SLP[2] =  opers[OAR[0]](SLP[1],  SLP[a]);
  for(b = 0; b <= 2;  b++){
  SLP[3] =  opers[OAR[1]](SLP[2],  SLP[b]);
  for(c = 0; c <= 3;  c++){
  SLP[4] =  opers[OAR[2]](SLP[3],  SLP[c]);
  for(d = 0; d <= 4;  d++){
  SLP[5] =  opers[OAR[3]](SLP[4],  SLP[d]);
  for(e = 0; e <= 5;  e++){
  SLP[6] =  opers[OAR[4]](SLP[5],  SLP[e]);
  for(f = 0; f <= 6;  f++){
  SLP[7] =  opers[OAR[5]](SLP[6],  SLP[f]);
  for(g = 0; g <= 7;  g++){
  SLP[8] =  opers[OAR[6]](SLP[7],  SLP[g]);
  for(h = 0; h <= 8;  h++){
  SLP[9] =  opers[OAR[7]](SLP[8],  SLP[h]);
  for(i = 0; i <= 9;  i++){
  SLP[10] = opers[OAR[8]](SLP[9],  SLP[i]);
  for(j = 0; j <= 10; j++){
  SLP[11] = opers[OAR[9]](SLP[10], SLP[j]);
  for(k = 0; k <= 11; k++){
  SLP[12] = opers[OAR[10]](SLP[11], SLP[k]);
  for(l = 0; l <= 12; l++){
  SLP[13] = opers[OAR[11]](SLP[12], SLP[l]);
  for(m = 0; m <= 13; m++){
  SLP[14] = opers[OAR[12]](SLP[13], SLP[m]);

  if(SLP[RAW] == TARGET){
    cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\ntarget found: 1 2 ";
    for(int index = 2; index <= RAW; index++)
      printf("0x%llx ", SLP[index]);
    cout << endl;
    return true; }}}}}}}}}}}}}}
  return false;
}

void next_ops(){
  if(OAR2[0] == OC) throw 0;
  memcpy(OAR, OAR2, sizeof(int) * (RAW - 1));
                 OAR2[12]++; if(OAR2[12] == OC){
    OAR2[12] = 0; OAR2[11]++; if(OAR2[11] == OC){
      OAR2[11] = 0; OAR2[10]++; if(OAR2[10] == OC){
        OAR2[10] = 0; OAR2[9]++; if(OAR2[9] == OC){
          OAR2[9] = 0; OAR2[8]++; if(OAR2[8] == OC){
            OAR2[8] = 0; OAR2[7]++; if(OAR2[7] == OC){
              OAR2[7] = 0; OAR2[6]++; if(OAR2[6] == OC){
                OAR2[6] = 0; OAR2[5]++; if(OAR2[5] == OC){
                  OAR2[5] = 0; OAR2[4]++; if(OAR2[4] == OC){
                    OAR2[4] = 0; OAR2[3]++; if(OAR2[3] == OC){
                      OAR2[3] = 0; OAR2[2]++; if(OAR2[2] == OC){
                        OAR2[2] = 0; OAR2[1]++; if(OAR2[1] == OC){
                          OAR2[1] = 0; OAR2[0]++; }}}}}}}}}}}}
}

void show_ops(){
  for(int i = 0; i < RAW - 1; i++)
    cout << annots[OAR[i]];
}

int main(int argc, char* argv[]){
  if(argc < 2){
    cout << "usage: < " << argv[0] << " n > to search for n!" << endl;
    return -1;
  }

  opers[0] = &Ot;
  opers[1] = &Op;
  opers[2] = &Om;

  TARGET = fact(atoi(argv[1]));

  environment env(argc, argv);
  communicator world;

  memset(&SLP, 0, (RAW + 1) * sizeof(LI));
  SLP[0] = 1; SLP[1] = 2;
  memset(&OAR, 0, (RAW - 1) * sizeof(int));
  memset(&OAR2, 0, (RAW - 1) * sizeof(int));

  if(world.rank() == 0){
    int slaves = world.size() - 1;
    request reqs[slaves];
    try{
      for(int rank = 1; rank < world.size(); rank++){
        next_ops(); show_ops(); cout << " to rank " << rank << endl;
        world.isend(rank, 0, OAR, RAW - 1);
        reqs[rank - 1] = world.irecv(rank, 1);
      }
      while(true){
        for(int i = 0; i < slaves; i++){
          auto opstatus = reqs[i].test();
          if(opstatus){
            next_ops(); show_ops(); cout << " to rank " << (i + 1) << endl;
            world.isend(i + 1, 0, OAR, RAW - 1);
            reqs[i] = world.irecv(i + 1, 1);
          }
        }
        usleep(50);
      }
    }catch(int){
      cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~\noperator products exhausted\n~~~~~~~~~~~~~~~~~~~~~~~~~~~"  << endl;
      wait_all(reqs, reqs + slaves);
      world.abort(0);
    }
  }else{
    while(true){
      world.recv(0, 0, OAR, RAW - 1);
      if(checkOperList()){
        cout << "opers: ";
        for(int i = 0; i < RAW - 1; i++)
          cout << annots[OAR[i]];
        cout << "\n~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
        world.abort(1);
      }
      world.send(0, 1);
    }
  }
  return 0;
}
{% endhighlight %}

Compile this code with  
`g++ -std=c++11 -O3 -c slp-dfs-14.cpp -o slp-dfs-14.o`

and link with

`g++ -lboost_serialization -lboost_system -lboost_mpi slp-dfs-14.o -o slp-dfs-14`

(or do it in one step, if you prefer).

## Results

I computed factorials from <span>\( 5! \)</span> to <span>\( 19! \)</span>. The above program was not used in the search, since it searches among SLP's of length <span>\( 14 \)</span>, but <span>\( 19! \)</span> has a length of <span>\( 13 \)</span>. The interpretation is that the intermediate result, is the result of the operator below it evaluated at the number before it, and at another number, that is somewhere in the list. Take <span>\( 5! \)</span> for example: <span>\( 120 = 128 - 8 \)</span>, <span>\( 128 = 16 * 8 \)</span>, <span>\( 16 = 8 * 2 \)</span>, <span>\( 8 = 4 * 2 \)</span> and <span>\( 4 = 2 * 2 \)</span>.

To get a sense of the size of the search space, the similar code to exhaustively search through all SLP's of length <span>\( 13 \)</span> took around 12 hours on a cluster of 256 cores of Xeon L5420 cpu's running at 2.50GHz.

* <span>\( 5!, N = 6 \)</span>
  <pre>
  1 2 4 8 16 128 120
    + * * *  *   -
  </pre>
* <span>\( 6!, N = 6 \)</span>
  <pre>
  1 2 4 16 20 36 720
    + * *  +  +  *
  </pre>
* <span>\( 7!, N = 7 \)</span>
  <pre>
  1 2 4 16 64 80 5120 5040
    + * *  *  +  *    -
  </pre>
* <span>\( 8!, N = 8 \)</span>
  <pre>
  1 2 4 8 12 144 136 280 40320
    + * * +  *   -   +   *
  </pre>
* <span>\( 9!, N = 8 \)</span>
  <pre>
  1 2 4 8 64 72 70 5040 362880
    + * * *  +  -  *    *
  </pre>
* <span>\( 10!, N = 9 \)</span>
  <pre>
  1 2 4 16 64 60 3840 3780 226800 3628800
    + * *  *  -  *    -    *      *
  </pre>
* <span>\( 11!, N = 9 \)</span>
  <pre>
  1 2 3 9 81 80 77 6160 492800 39916800
    + + * *  -  -  *    *      *
  </pre>
* <span>\( 12!, N = 10 \)</span>
  <pre>
  1 2 4 6 24 576 600 3600 21600 22176 479001600
    + * + *  *   +   *    *     +     *
  </pre>
* <span>\( 13!, N = 11 \)</span>
  <pre>
  1 2 4 16 256 272 288 78336 78624 78912 79200 6227020800
    + * *  *   +   +   *     +     +     +     *
  </pre>
* <span>\( 14!, N = 11 \)</span>
  <pre>
  1 2 4 16 20 22 42 840 13440 295680 294840 87178291200
    + * *  +  +  +  *   *     *      -      *
  </pre>
* <span>\( 15!, N = 12 \)</span>
  <pre>
  1 2 4 8 32 36 28 1008 1000 36000 36036 36324288 1307674368000
    + * * *  +  -  *    -    *     +     *        *
  </pre>
* <span>\( 16!, N = 12 \)</span>
  <pre>
  1 2 4 8 12 144 1152 1008 145152 144000 145152000 145297152 20922789888000
    + * * +  *   *    -    *      -      *         +         *
  </pre>
* <span>\( 17!, N = 12 \)</span>
  <pre>
  1 2 4 6 36 1296 1260 5040 3744 3740 18849600 95001984000 355687428096000
    + * + *  *    -    *    -    -    *        *           *
  </pre>
* <span>\( 18!, N = 13 \)</span>
  <pre>
  1 2 4 6 36 72 108 112 8064 7956 891072 891000 7185024000 6402373705728000
    + * + *  *  +   +   *    -    *      -      *          *
  </pre>
* <span>\( 19!, N = 13 \)</span>
  <pre>
  1 2 4 6 24 576 600 2400 2394 2970 7128000 7128576 17065810944 121645100408832000
    + * + *  *   +   *    -    +    *       +       *           *
  </pre>
