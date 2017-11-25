![MIT](https://badges.frapsoft.com/os/mit/mit.svg?v=102)

## Intro
This chatbot is trained by seq2seq model described in [Sequence to Sequence -- Video to Text](https://arxiv.org/abs/1505.00487).

Then use the reward function described in [Deep Reinforcement Learning for Dialogue Generation](https://arxiv.org/abs/1606.01541) to increase the performance.

<img align='center' style="border-color:gray;border-width:2px;border-style:dashed"   src='https://github.com/brianhuang1019/Reinforcement-Learning_Chatbot/blob/master/RL-chatbot.png' padding='5px' height="200px"></img>
<h6><a href='https://phrasee.co/wp-content/uploads/2016/08/reinforcement-learning.gif'>Image src</a></h6>



## chatbot's results

Let me show you some results below:

* __Result 1__ (A & B are both a trained chatbot)
>A: Can you do me a favor?

>B: Anything.

>A: I want you to bend a father.

>B: Of course.

>A: I'm so sorry I didn't mean to be rude but.

>B: Don't get it at least it's about as much as I can.

* __Result 2__ (A & B are both a trained chatbot)
>A: I didn't do it! I haven't got a gun!

>B: You liar. You still want to get your money back.

>A: Something wrong with the truth.

>B: I don't want any money just a little I can't take just out of this.

>A: What's wrong.

>B: Listen to me. I'm scared now.

>A: Ah you'll need all the money I need your help.

* __Result 3__ (A & B are both a trained chatbot)
>A: I forgot to get the Coca-Cola.

>B: I got something stuck in the head.

>A: It's all right I guess it's pretty common in the lot of shit.

>B: I mean we've been all this together since the kid.

>A: All the more reason.

>B: It's not him it's his fault and he's blind because of god.

>A: And is a strange story.

you can find more results from chatbot in *tmp/* directory

neural networks' configs of those results are described in the filename



## simulate a dialog by pre-trained model
Are you amazed at the result of the chatbot? :D

You can generate those amazing dialogs by your own!

Just follow the instructions below:

```bash
./script/download.sh
```
###### (Mention that I use -nc parameter in script/download.sh, it will omit downloading if the file exists. So make sure there's no break during the download)
```bash
./script/simulate.sh <PATH TO MODEL> <SIMULATE TYPE> <INPUT FILE> <OUTPUT FILE>
```

- \<PATH TO MODEL\>

to generate seq2seq dialog, type "model/Seq2Seq/model-77"

to generate RL dialog, type "model/RL/model-56-3000"

- \<SIMULATE TYPE\> 

can be 1 or 2

the number represents # of former sentence(s) that chatbot considers

__if you choose 1, chatbot only considers user's utterance__

__if you choose 2, chatbot will considers user's utterance and chatbot itself's last utterance__

- \<INPUT FILE\>

Take a look at [example file](tmp/sample_input_new.txt) 

This is the input format of the chatbot, each line is the begin sentence of a dialog.

You can just use the example file for convenience.

- \<OUTPUT FILE\> 

the output file, type any filename you want



## generate response by pre-trained model
If you want chatbot to generate only a single response

Follow the instructions below:

```bash
./script/download.sh
```
###### (Mention that I use -nc parameter in script/download.sh, it will omit downloading if the file exists. So make sure there's no break during the download)
```bash
./script/run.sh <TYPE> <INPUT FILE> <OUTPUT FILE>
```

- \<TYPE\> 
can be one of below:
1. S2S
2. RL

- \<INPUT FILE\> 

Take a look at [example file](tmp/sample_input_new.txt) 

This is the input format of the chatbot, each line is the begin sentence of a dialog.

You can just use the example file for convenience.

- \<OUTPUT FILE\> 

the output file, type any filename you want



## train chatbot from scratch
I trained my chatbot with python2.7.

If you want to train the chatbot from scratch

You can follow those instructions below:

#### Step0: training configs
Take a look at [python/config.py](python/config.py), all configs for training is described here.

You can change some training hyper-parameters, or just keep the original ones.

#### Step1: download data & libraries
I use [Cornell Movie-Dialogs Corpus](https://www.cs.cornell.edu/~cristian/Cornell_Movie-Dialogs_Corpus.html)

You need to download it, unzip it, and move all .txt files into data/ directory

Then download some libraries with pip:
```bash
pip install -r requirements.txt
```

#### Step2: parse data 
###### (in this step I use python3)
```bash
./script/parse.sh
```

#### Step3: train a Seq2Seq model
```bash
./script/train.sh
```

#### Step4-1: test a Seq2Seq model
Let show some result of seq2seq model :)
```bash
./script/test.sh <PATH TO MODEL> <INPUT FILE> <OUTPUT FILE>
```

#### Step4-2: simulate a dialog
And show some dialog result of seq2seq model
```bash
./script/simulate.sh <PATH TO MODEL> <SIMULATE TYPE> <INPUT FILE> <OUTPUT FILE>
```
- \<SIMULATE TYPE\> 

can be 1 or 2

the number represents # of former sentence(s) that chatbot considers

if you choose 1, chatbot will only considers user's utterance

if you choose 2, chatbot will considers user's utterance and chatbot's last utterance

#### Step5: train a RL model
you need to change the *training_type* parameter in [python/config.py](python/config.py)

'normal' for seq2seq training, 'pg' for policy gradient

you need to first train with 'normal' for some epochs till stable (at least 30 epoches is highly recommended)

then change the method to 'pg' to optimize the reward function

```bash
./script/train_RL.sh
```

*When training with policy gradient (pg)*

*you may need a reversed model*

*you can train it by your-self*

*or you can download pre-trained reversed model by*
```bash
./script/download_reversed.sh
```

*the reversed model is also trained by cornell movie-dialogs dataset, but with source and target reversed.*

*you don't need to change any setting about reversed model if you use pre-trained reversed model*

#### Step6-1: test a RL model
Let show some result of RL model, and find the different from seq2seq model :)
```bash
./script/test_RL.sh <PATH TO MODEL> <INPUT FILE> <OUTPUT FILE>
```

#### Step6-2: simulate a dialog
And show some dialog result of RL model
```bash
./script/simulate.sh <PATH TO MODEL> <SIMULATE TYPE> <INPUT FILE> <OUTPUT FILE>
```
- \<SIMULATE TYPE\> 

can be 1 or 2

the number represents # of former sentence(s) that chatbot considers

if you choose 1, chatbot will only considers user's utterance

if you choose 2, chatbot will considers user's utterance and chatbot's last utterance
