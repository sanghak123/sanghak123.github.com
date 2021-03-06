---
layout: post
title: "Understanding LSTM Networks"
date: "2018-08-01 02:24:13 +0900"
---
<script src="//cdnjs.cloudflare.com/ajax/libs/mathjax/2.5.3/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

## Recurrent Neural Networks
LSTM은 RNN의 한 종류인데, 기존의 RNN보다 성능이 월등히 좋다. RNN을 통해서 얻어낸 업적들의 대부분은 LSTM을 통해서 얻어냈다고 해도 과언이 아니다.

## The Problem of Long-Term Dependencies
RNN의 의의는 과거의 정보를 현재 일에 사용할 수 있다는 데에 있다.  
“the clouds are in the \_\_\_\_” 와 같은 문장에서 sky를 찾는 것은 최근 등장한 단어들을 통해서 유추하면 되므로 매우 쉽다. 하지만, “I grew up in France… I speak fluent \_\_\_\_” 와 같은 문장에서 French를 찾는 것은 어렵고 매우 멀리에 있는 정보를 통해서 얻어야한다는 문제가 있다. 하지만 RNN은 이렇게 매우 멀리에 있는 정보를 연결시킬 수 없다.  
이론상으론, RNN은 'long-term dependencies'를 처리할 수 있다곤 하지만, 실제 문제에서는 한계점을 보인다. 이런 문제를 __LSTM__ 이 해결할 수 있다고 한다!

## LSTM Networks
앞서 말했듯이 __LSTM__ 은 'long-term dependencies'를 해결할 수 있고, 그래서 많이 쓰이고 있으며, 좋은 성능을 보이고 있다.  
긴 시간동안 정보를 기억하는 것이 LSTM의 가장 중요한 특징이다.  
![RNN overview]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-SimpleRNN.png" | absolute_url }})  
일반적인 RNN의 구조는 위와 같고, 루프 부분은  $$tanh$$ 레이어로만 구성되어있다.  
![LSTM overview]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-chain.png" | absolute_url }})  
반면에 LSTM의 구조는 위와 같고, 4개의 레이어로 구성되어있다.

## The Core Idea Behind LSTMs
LSTM에서 가장 중요한 것은 __cell state__ 또는 __c-line__ 이라고 하는 것인데, 이것은 위에서 봤던 그림에서 가로로 지나가는 선을 말한다. 그림으로 보면 다음과 같다.  
![Cell state line]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-C-line.png" | absolute_url }})  
$$C_{t-1}$$에서 $$C_t$$로 흘러가는 라인인데, 여기에 몇가지 데이터의 수정을 거치며 흘러간다.  
또 그림을 보면 다음과 같은 구조도 등장하는데,  
![Gate]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-gate.png" | absolute_url }})  
이것을 __gate__ 라고 부르며, 앞서 봤던 c-line에 데이터 수정을 해주는 장치라고 생각하면 된다.  
$$\sigma$$는 sigmoid 함수로서, 아래에서 올라오는 값을 0과 1 사이의 값으로 만들어준다. 라인 중간에 있는 X표 동그라미는 pointwise multiplication operator인데, 이것이 sigmoid와 함께 사용되어, 아래에서 올라오는 값을 토대로, c-line 왼쪽에서 오른쪽으로 얼마나 전달할지를 결정한다고 생각하면 된다.

결국 LSTM은 3개의 서로 다른 gate를 통해서 cell state의 값을 유지하고, 조절한다는 것을 알 수 있다.

## Step-by-Step LSTM Walk Through
### Forget Gate  
![Forget gate]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-f.png" | absolute_url }})     
첫번째 게이트는 forget gate인데, 어떤 정보를 버릴지 정하는 게이트이다. 이전 loop iteration의 결과값인 $$h_{t-1}$$와 새로운 인풋인 $$x_t$$를 받아서 forget gate에 해당하는 weight $$W_f$$ 를 곱해주고 이것을 sigmoid 처리함으로써 0과 1 사이의 값을 갖게 한 다음 cell state에 pointwise multiplication 해준다. 여기에서 sigmoid의 결과값이 0에 가까울수록 더 많이 잊는다.

뒤에 올 단어를 찾는 모델을 구성할 때, 주어의 성별이 중요한 경우 이 성별을 계속 기억을 해두었다가, 나중에 주어가 바뀐 경우 이전의 성별을 잊기 위해서 어떤 operation을 진행해야하는데 forget gate가 이것을 해주게 된다.
## Input Gate and $$tanh$$  
![Input gate]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-i.png" | absolute_url }})  
sigmoid layer는 input gate라고 하고, 어떤 값을 업데이트 할지 고르고, $$tanh$$는 새로운 후보 값인 $$\tilde{C}_t$$를 만든다. 그리고 두 값을 결합하고, 이것을 가지고 cell state를 업데이트 해준다.

앞의 예에서 새 주어의 성별 정보를 얻어야 할 때 이 게이트를 사용한다.
## New Cell State  
![Input gate]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-C.png" | absolute_url }})  
$$f_t$$, $$i_t$$를 사용하여 cell state를 업데이트 해준다. 이전 state를 얼마나 기억할지를 $$f_t$$를 통해서 결정하고, 새로운 후보 state $$\tilde{C}_t$$와 이것을 얼마나 적용할 지를 $$i_t$$를 곱한 후 두 값을 합한다.
## New Output  
![Output]({{ "http://colah.github.io/posts/2015-08-Understanding-LSTMs/img/LSTM3-focus-o.png" | absolute_url }})  
새로 계산된 cell state에서 어떤 값을 쓸지를 결정하는 sigmoid layer, cell state에 $$tanh$$를 처리한 layer. 그 두 레이어의 값을 곱해서 그것을 새로운 output인 $$h_t$$로 쓴다.
