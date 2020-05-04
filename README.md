# Instructions for basic use:



## Install the dependencies

`pip3 install tensorflow-gpu==1.14 progressbar numpy scipy pandas python_speech_features tables attrdict pyxdg`



## Clone the Mozilla DeepSpeech repository into a folder called DeepSpeech:

`git clone https://github.com/mozilla/DeepSpeech.git`

` git checkout tags/v0.4.1`

- Checkout the correct version of the code:

`cd DeepSpeech;`

`pip3 install $(python3 util/taskcluster.py --decoder)`  

- if the above code is running incorrectly,follow these steps

`make -C native_client/ctcdecode`

`cd native_client/ctcdecode/dist; `

`pip install .whl`



## Download the DeepSpeech model

`wget https://github.com/mozilla/DeepSpeech/releases/download/v0.4.1/deepspeech-0.4.1-checkpoint.tar.gz`
`tar -xzf deepspeech-0.4.1-checkpoint.tar.gz`

- these is baidunetdiskdownload link

`hold on`



## Verify that you have a file deepspeech-0.4.1-checkpoint/model.v0.4.1.data-00000-of-00001
Its MD5 sum should be `ca825ad95066b10f5e080db8cb24b165`



## Check that you can classify normal images correctly

python3 classify.py --in sample-000000.wav --restore_path deepspeech-0.4.1-checkpoint/model.v0.4.1



## Generate adversarial examples

python3 attack.py --in sample-000000.wav --target "this is a test" --out adv.wav --iterations 1000 --restore_path deepspeech-0.4.1-checkpoint/model.v0.4.1



## Verify the attack succeeded

python3 classify.py --in adv.wav --restore_path deepspeech-0.4.1-checkpoint/model.v0.4.1



# Related content

This is the code corresponding to the paper
"Audio Adversarial Examples: Targeted Attacks on Speech-to-Text"
Nicholas Carlini and David Wagner
https://arxiv.org/abs/1801.01944

To generate adversarial examples for your own files, follow the below process
and modify the arguments to attack,py. Ensure that the file is sampled at
16KHz and uses signed 16-bit ints as the data type. You may want to modify
the number of iterations that the attack algorithm is allowed to run.

WARNING: THIS IS NOT THE CODE USED IN THE PAPER. If you just want to get going
generating adversarial examples on audio then proceed as described below.

The current master branch points to code which will run on TensorFlow 1.14 and
DeepSpeech 0.4.1, an almost-recent version of the dependencies. (Large portions
of tf_logits.py will need to be re-written to run on DeepSpeech 0.5.1 which uses
a new feature extraction pipeline with TensorFlow's C++ implementation. If you
feel motivated to do that I would gladly accept a PR.)

However, IF YOU ARE TRYING TO REPRODUCE THE PAPER (or just have decided
that you enjoy pain and want to suffer through dependency hell) then you
will have to checkout commit a8d5f675ac8659072732d3de2152411f07c7aa3a and
follow the README from there.

