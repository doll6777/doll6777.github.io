---
layout: post
title: SeedBank의 Char RNN 분석 (RNN과 LSTM)
excerpt: "Deep Learning"
tags: [deep learning, programming, dl]
permalink: /deep-learning/:year/:month/:day/:title/
category : 딥러닝
---

딥러닝 스터디를 진행하며 SeedBank의 Char-RNN에 대하여 발표하게 되었다. SeedBank 에서는 다양한 딥러닝 프로젝트들을 웹상에서 실행 가능하고 학습할 수 있다.

> https://research.google.com/seedbank/seed/charrnn

# Char-RNN
문자열을 사용하여 문자열을 생성해낸다. 이것을 RNN을 사용하여 구현할 수 있다. RNN이 뭔지 먼저 알아보자.

# RNN(Recurrent Neural Network)
RNN은 시퀀스 데이터를 처리하는 모델이다. 시퀀스란 순서대로 처리하는 것을 뜻하는데, 특히 음성인식이나 자연어처리 등이 포함된다. 이런 데이터들은 앞뒤 문맥을 이해해야 해석할수 있기 때문이다.  

![image](/assets/2019-10-02-char-rnn-seedbank/diags.jpeg)

RNN의 가장 간단한 모델은 바닐라(Vanilla) 모델이다. 이는 one-to-one 모델이다. 

![image](/assets/2019-10-02-char-rnn-seedbank/2.png)
![image](/assets/2019-10-02-char-rnn-seedbank/1.png)

하나의 입력을 받으면 h라는 상태에 값을 저장해두고, 다음 노드의 값을 계산할 때 이전에 저장했던 상태 h의 값을 적용해서 계산한다.  

# LSTM이란?
RNN은 관련 정보와 그 정보를 사용하는 지점 사이 거리가 멀 경우 그래디언트가 점점 줄어 학습능력이 크게 저하되는 것으로 알려져 있다.  
이를 Vanishing Gradient Problem 이라고 한다. 이 문제를 극복하기 위해서 고안된 것이 LSTM 이다.  

# Vanishing Gradient Problem
![image](https://www.google.com/url?sa=i&source=images&cd=&cad=rja&uact=8&ved=2ahUKEwjl3sGW3a3mAhVTFYgKHevQCsgQjRx6BAgBEAQ&url=https%3A%2F%2Fsmartstuartkim.wordpress.com%2F2019%2F02%2F09%2Fvanishing-gradient-problem%2F&psig=AOvVaw1nU_bH-mdIR4BaSinNiUIc&ust=1576155818967974)


> LSTM이란 RNN의 hidden state에 cell-state를 추가한 구조이다.
![image](/assets/2019-10-02-char-rnn-seedbank/3.png)

# Char-RNN 실행해보기
> <https://colab.research.google.com/drive/13Vr3PrDg7cc4OZ3W2-grLSVSf0RJYWzb#forceEdit=true&offline=true&sandboxMode=true>

코드는 위 url을 클릭하면 확인할 수 있다. RNN은 tensorflow를 사용한다. 셰익스피어의 소설을 인풋으로 받고 학습시키면, 입력된 문자열에 따라 문장을 생성해낸다. 이때 임베딩이라는 것이 나오는데,

# Embedding
단어를 벡터로 표현하는 것이다. 이를 사용하면 범주형 자료를 연속적으로 표현할 수 있다. 범주형 자료를 one-hot encoding으로 표현하면 n-1 차원이 필요하지만, enbedding을 사용하면 2, 3차원으로도 표현 가능하다.  

vector space로 표현하면 의미를 도출하기 쉬워진다는 장점도 있다.  

가장 대표적인 알고리즘은 Word2vec 이다.  

<pre class="prettyprint">
  def _build_rnn(self, inputs, dropout_rate):
    """Generates an RNN model using the passed functions.

    Args:
      inputs: int32 Tensor with shape [batch_size, sequence_length] containing
        input labels.
      dropout_rate: A floating point value determining the chance that a weight
        is forgotten during evaluation.
    """
    # Alias some commonly used functions
    dropout_wrapper = tf.contrib.rnn.DropoutWrapper
    lstm_cell = tf.contrib.rnn.LSTMCell
    multi_rnn_cell = tf.contrib.rnn.MultiRNNCell

    self._cell = multi_rnn_cell(
        [dropout_wrapper(lstm_cell(self.state_size), 1.0, 1.0 - dropout_rate)
         for _ in range(self.num_layers)])

    self.initial_state = self._cell.zero_state(self.batch_size, tf.float32)

    embedding = tf.get_variable('embedding',
                                [self.num_classes, self.state_size])

    embedding_input = tf.nn.embedding_lookup(embedding, inputs)
    output, self.new_state = tf.nn.dynamic_rnn(self._cell, embedding_input,
                                               initial_state=self.initial_state)

    self.logits = tf.contrib.layers.fully_connected(output, self.num_classes,
                                                    activation_fn=None)
</pre>

# Seq2Seq
Encoder와 Decoder로 이루어져 있고, 인코더를 지나 ContextVector에 문맥이 저장되며 이러부터 Decoder가 기계 번역을 시작한다. 

<pre class="prettyprint">
def get_loss(logits, targets, target_weights):
  with tf.name_scope('loss'):
    return tf.contrib.seq2seq.sequence_loss(
        logits,
        targets,
        target_weights,
        average_across_timesteps=True)
</pre>

코랩에서의 인풋은 다음과 같다.
```
An excerpt: 
                      1
  From fairest creatures we desire increase,
  That thereby beauty's rose might never die,
  But as the riper should by time decease,
  His tender heir might bear his memory:
  But thou contracted to thine own bright eyes,
  Feed'st thy light's flame with self-substantial fuel,
  Making a famine where abundance lies,
  Thy self thy foe, to thy sweet self too cruel:
  Thou that art now the world's fresh ornament,
  And only herald to the gaudy spring,
  Within thine own bud buriest thy content,
  And tender churl mak'st waste in niggarding:
    Pity the world, or else this glutton be,
    To eat the world's due, by the grave and thee.

```
아웃풋은 다음과 같다.
```
ANTONIO: Who is it ?IE. Do piy min, wher by till blingestn.
    Mave in wind for ne cient hafesteres one for yor your yaev yould and londond, afrely,,
    At that Wood tho your you, waecth wing Cichbry your,
    I theer mien, ward the me a see hen thy yould berliiver to her mesting of rive ore yout,
     Thoush F'er in helr all hive und so bing the nost atHer with stourss the madss'
    Kom no, you shat with thing in shee and wear lat yom horch.
    Aqhat wirte you dose ou? 'con acrerante,
    Fow his nast sthe, lost the sarthind.
    Read und is liubk.                                                                         Anmen swenely they for musmer, with the woruster. Aurles I I hell with she laikn.
    Mush me  tid. Why will nle bes by lothile Vith is entern-that recume thee, anlind on whou to rudes
    Bust a hath tull a pnane form gowet proncud, here and thou theat sam,
    Tull at the me then with you sowe braen wune
    Susralshes to gresheneuy, and tleacenss
    thall a the pids my goon and so me els coon fathire
    And prove, but anderers
    will for gromen hit, suik, in that, food of thee shoundly;
    Luttarr my tutt frat yoe?
  NUBITORSNO. What whe vay ip withel. Sretels ase lotst a pold,
    The shigh old hath not swamure to have
    And your for youl cuetin serserby,
    And gofe wham wis it whoud thith hesw of in wour.
  COLID. For at tall yourd and shaede no hach bethe,
    and that maty tood beulder, arld me sende ingee wit te dreike
    I to seever four bud so hen? cithrer, lesthitee. worlmy me thave peralion;
    Gut be's, hiw woR brafans on now shat blomelf wouk,
    Home sithetht
    To dutwell wills in wo tege me noptnes?
    Whaw so fallers, doed athise sist to mech not on, hither we 'tund, thoun
    Noald of wamst is stinle broulk.
  HACATIS. Hy weach,
    Hithen coluntange bryake, there your wast whave borje ould. I tot at
    Sore his the lat apowine i  mest come.
  POCKEN. Yich to shaghed as sele the, youd peesiter be, is now rasol.
```

