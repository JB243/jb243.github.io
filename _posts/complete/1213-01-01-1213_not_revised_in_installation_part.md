## **Installing and Understanding AlphaGeometry**

Recommended post: 【Algorithm】 [Algorithm & Machine Learning Index](https://jb243.github.io/pages/1278)

---

**1.** [Overview](#1-overview)

**2.** [Installation Process](#2-installation-process)

**3.** [Syntax](#3-syntax)

---

**a.** [IMO Geometry Problem Solving](https://jb243.github.io/pages/457)

**b.** [Reconstruction and Solution of jgex_ag_231 Problem](https://jb243.github.io/pages/2291)

**c.** [Natural Language Processing and Large Language Models](https://jb243.github.io/pages/325)

---

<br>

## **1. Overview**

⑴ Introduction

> ① [AlphaGeometry](https://www.nature.com/articles/s41586-023-06747-5): A language model submitted to a Nature publication that implements symbolic deduction, capable of solving geometry problems at the level of the International Mathematical Olympiad (IMO).

> ② Developed by Google DeepMind, it appears to be an advancement of the hypothesis-generating model [FunSearch](https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/funsearch-making-new-discoveries-in-mathematical-sciences-using-large-language-models/Mathematical-discoveries-from-program-search-with-large-language-models.pdf). ([ref](https://jb243.github.io/pages/2379))

⑵ Principle

> ① Meaning of a symbol: The process of vectorizing each proposition and placing it on an embedding space.

> ② Symbolic deduction in geometry: Similar to "Euclid's Elements," it starts with axioms about lines, constructions, and concentric circles, leading to the creation of new propositions.

> ③ When given the conditions of a problem, it continuously searches for new true propositions within the search space to prove the given propositions.

> ④ This process can be likened to how a person might ponder a math problem for a long time before finally discovering the solution.

> ⑤ More specifically, 1) it represents the given geometry problem as points, lines, and surfaces, 2) creates a graph data structure, and then 3) operates by connecting the graph data structure with the theorem set.

>> ○ In that case, is it possible to represent medical data as a graph data structure, connect it with clinical information, and implement a logical inference model? (`[The Present and Future of the Bio Industry](https://jb243.github.io/pages/2411)`)

⑶ Process

> ① AlphaGeometry primarily solves problems through angle chasing.

>> ○ Many geometry problems can be solved by angle chasing. For example, the proposition that four points lie on a circle (cyclic proposition) is equivalent to the condition that the circumferential angles of two triangles sharing the same base are equal.

> ② **Step 1.** DD+AR: Repeated use of cyclic quadrilaterals and similar triangles in geometry, more akin to classical algorithms than to AI ([ref](https://www.reddit.com/r/math/comments/19fg9rx/some_perspective_on_alphageometry/?onetap_auto=true&one_tap=true)).

> ③ **Step 2.** AlphaGeometry mode: Adds new points randomly and then performs DD+AR, using an LLM-based approach.

>> ○ The addition of new points is a heuristic process in humans, now made possible creatively with the advent of transformer-based LLMs.

⑷ Limitations

> ① **Limitation 1.** While geometric problems are based on definitive propositions leading to logical conclusions, real-world problems often depend on indeterminate propositions for decision-making.

> ② **Limitation 2.** Geometric problems often involve equivalence issues, whereas real-world decision-making frequently involves comparative issues.

>> ○ Example of equivalence issue: Being on the same circle, M is the midpoint of A and B, etc.

>> ○ Example of comparative issue: Will treating a breast cancer patient with Gemcitabine increase survival rates?

>> ○ Can AlphaGeometry, which solves equality problems, solve comparison problems given the intrinsic limitations of triangles?

> ③ **Limitations 3.** Geometric problems deal with bidirectional propositional relationships (where two propositions are necessary and sufficient conditions for each other), but in real-world decision-making, there are many unidirectional propositional relationships.

>> ○ In other words, if A → B holds, then in geometric problems B → A also holds. However, in real-world decision-making, it is often the case that B ⇏ A.

>> ○ Similarly, if A → B holds, then in geometric problems ~A → ~B also holds. However, in real-world decision-making, it is often the case that ~A ⇏ ~B.

⑸ Significance

> ① **Significance 1.** The first model for logical reasoning. (`[AI That Generates Hypotheses: The Beginning of the Idea](https://jb243.github.io/pages/2379)`)

> ② **Significance 2.** Possibility for logical reasoning on a large scale: If solving an IMO problem involves linking dozens of propositions, could humanity connect over a thousand propositions to derive new conclusions through AI? This may be possible through AI, marking a breakthrough in the horizons of cognition. (`[The Horizon of Cognition and the Breakthrough Called AI](https://jb243.github.io/pages/2410)`)

> ③ **Significance 3.** Even if there is an incorrect proposition, it does not affect the final logical reasoning.

<br>

<br>

## **2. Installation Process**

⑴ The procedure in the [official documentation](https://github.com/google-deepmind/alphageometry) causes errors, so follow these steps instead.

⑵ **Step 1.** Execute the following on an Ubuntu server: The author uses Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-105-generic x86_64).

<br>

```python
sudo apt install python3-virtualenv
conda create -n alphageometry python=3.10.9
conda activate alphageometry
git clone https://github.com/google-deepmind/alphageometry.git
cd alphageometry
pip install array-record==0.4.1
pip install clu==0.0.7
pip install seqio==0.0.18
pip install seqio-nightly==0.0.17.dev20231013 
pip install t5==0.9.4
pip install tensorflow-datasets==4.9.3
pip install mesh-tensorflow[transformer]==0.1.21
pip install tfds-nightly==4.9.2.dev202308090034
```

<br>

⑶ **Step 2.** Remove the following installation items from requirements.txt:

> ① array-record  

> ② clu  

> ③ seqio  

> ④ seqio-nightly  

> ⑤ t5  

> ⑥ tensorflow-datasets  

> ⑦ mesh-tensorflow[transformer]  

> ⑧ tfds-nightly

⑷ **Step 3.** `bash run.sh`: Although this code line skips much of steps 1 and 2, it causes package errors.

> ① An error occurs at `gdown --folder https://bit.ly/alphageometry` when executing the code.

> ② **Troubleshooting:** It might be worth trying to install using the following [run.sh](https://blog.kakaocdn.net/dn/bvifig/btsITupiwRM/kkBeS5szIKKVLIJZhMKO5k/run.sh?attach=1&knm=tfile.sh), referring to [salihmarangoz's commit](https://github.com/google-deepmind/alphageometry/pull/54/commits/3debb824a1c199c789fb8d4f562dc1319ac9c6c8) (although an error occurs).

⑸ **Step 4.** Download checkpoint files from the following links and place them in the `ag_ckpt_vocab` folder:

> ① `checkpoint_10999999`: <https://drive.google.com/uc?id=1qXkmmgoJ8oTYJdFV1xw0xGPpQj6SyOYA>  

> ② `geometry.757.model`: <https://drive.google.com/uc?id=1t-r3KfU8aDbS1UHpdyM3LH21rwSCIXTz>  

> ③ `geometry.757.vocab`: <https://drive.google.com/uc?id=1mRd6J0UkeWoFUjeVB7BQi5lVNLvPBe31>

⑹ **Step 5.** Execute the remaining code that could not be run in `bash run.sh`.

<br>

```python
MELIAD_PATH=meliad_lib/meliad
mkdir -p $MELIAD_PATH
git clone https://github.com/google-research/meliad $MELIAD_PATH
export PYTHONPATH=$PYTHONPATH:$MELIAD_PATH

DDAR_ARGS=(
  --defs_file=$(pwd)/defs.txt \
  --rules_file=$(pwd)/rules.txt \
);

BATCH_SIZE=2
BEAM_SIZE=2
DEPTH=2

SEARCH_ARGS=(
  --beam_size=$BEAM_SIZE
  --search_depth=$DEPTH
)

LM_ARGS=(
  --ckpt_path=$DATA \
  --vocab_path=$DATA/geometry.757.model \
  --gin_search_paths=$MELIAD_PATH/transformer/configs \
  --gin_file=base_htrans.gin \
  --gin_file=size/medium_150M.gin \
  --gin_file=options/positions_t5.gin \
  --gin_file=options/lr_cosine_decay.gin \
  --gin_file=options/seq_1024_nocache.gin \
  --gin_file=geometry_150M_generate.gin \
  --gin_param=DecoderOnlyLanguageModelGenerate.output_token_losses=True \
  --gin_param=TransformerTaskConfig.batch_size=$BATCH_SIZE \
  --gin_param=TransformerTaskConfig.sequence_length=128 \
  --gin_param=Trainer.restore_state_variables=False
);

echo $PYTHONPATH
```

<br>

⑺ **Step 6.** Installation is complete, but some errors remain, requiring the installation of additional packages.

<br>

```python
pip install matplotlib

sudo apt-get install xvfb
sudo apt-get install python3-tk
Xvfb :1 -screen 0 1024x768x16 &
export DISPLAY=:1.0

pip uninstall flax jax jaxlib
pip install flax==0.5.3 jax==0.4.6 jaxlib==0.4.6
```

<br>

⑻ **Step 7.** AlphaGeometry can now be executed in `ddar` mode.

<br>

```python
python -m alphageometry \
--alsologtostderr \
--problems_file=$(pwd)/imo_ag_30.txt \
--problem_name=translated_imo_2000_p1 \
--mode=ddar \
"./defs.txt"
```

<br>

⑼ **Step 8.** To run in alphageometry mode, move the following `.gin` files to `./`:

> ① Move `./meliad_lib/meliad/transformer/configs/base_htrans.gin` to `./`.  

> ② Move `./meliad_lib/meliad/transformer/configs/memory_configuration.gin` to `./`.  

> ③ Move `./meliad_lib/meliad/transformer/configs/trainer_configuration.gin` to `./`.

⑽ **Step 9.** Modify lax_numpy.py

> ① Issue Encountered: When running AlphaGeometry in alphageometry mode, the following error message occurs:

> ② Error Message: TypeError: JAX only supports number and bool dtypes, got dtype bfloat16 in astype, among 3 others

> ③ Solution: Modify using nano {path_to_lax_numpy}/lax_numpy.py as follows:

<br>

```python
# before
... (skip)
dtypes.check_user_dtype_supported(dtype, "astype")
... (skip)
dtypes.check_user_dtype_supported(dtype, "astype")
... (skip)
dtypes.check_user_dtype_supported(dtype, "asarray")
... (skip)
dtypes.check_user_dtype_supported(dtype, "array")
.. (skip)
dtypes.check_user_dtype_supported(dtype, "zeros")
... (skip)

# after
... (skip)
try:
    dtypes.check_user_dtype_supported(dtype, "astype")
except:
    dtype = dtypes.bfloat16
... (skip)
try:
    dtypes.check_user_dtype_supported(dtype, "astype")
except:
    dtype = dtypes.bfloat16
... (skip)
try:
    dtypes.check_user_dtype_supported(dtype, "asarray")
except:
    dtype = dtypes.bfloat16
... (skip)
try:
    dtypes.check_user_dtype_supported(dtype, "array")
except:
    dtype = dtypes.bfloat16
.. (skip)
try:
    dtypes.check_user_dtype_supported(dtype, "zeros")
except:
    dtype = dtypes.bfloat16
... (skip)
```

<br>

> ④ The modified lax_numpy.py file, which has been confirmed to function normally up to the midpoint in alphageometry mode, is as follows: <[lax_numpy.py](https://blog.kakaocdn.net/dn/bRf0Tg/btsH5LeWWOd/P1giKuX3ypGfQcQi2Oxk60/lax_numpy.py?attach=1&knm=tfile.py)>

⑾ **Step 10.** Copy the following file to the root directory, `/.`

> ① `checkpoint_10999999`

> ② `geometry.757.model`

> ③ `geometry.757.vocab`

⑿ **Step 11.** AlphaGeometry can now be executed in alphageometry mode.

<br>

```python
python -m alphageometry \
--alsologtostderr \
--problems_file=$(pwd)/imo_ag_30.txt \
--problem_name=translated_imo_2000_p6 \
--mode=alphageometry \
"./defs.txt" \
"${SEARCH_ARGS[@]}" \
"${LM_ARGS[@]}"
```

<br>

⒀ **Troubleshooting:** TypeError: cannot unpack non-iterable Point object

> ① Solution: Modify by using 'nano {path_to_numericals}/numericals.py' as follows:

<br>

```python
# before
def check_cyclic(points: list[Point]) -> bool:
  points = list(set(points))
  (a, b, c), *ps = points
  circle = Circle(p1=a, p2=b, p3=c)
  for d in ps:
    if not close_enough(d.distance(circle.center), circle.radius):
      return False
  return True
  
# after  
def check_cyclic(points: list[Point]) -> bool:
  try:
    points = list(set(points))
    (a, b, c), *ps = points
    circle = Circle(p1=a, p2=b, p3=c)
    for d in ps:
      if not close_enough(d.distance(circle.center), circle.radius):
        return False
  except:
    False
  return True
```

<br>

⒁ **Troubleshooting:** Editing the requirements.txt can be cumbersome.

> ① Solution: Modify run.sh as follows. ([ref](https://stackoverflow.com/questions/75729749/getting-error-in-require-hashes-mode-all-requirements-must-have-their-versi))

<br>

```python
# before
pip install --require-hashes -r requirements.txt 

# after
pip install --require-hashes --no-deps -r requirements.txt
```

<br>

⒂ **Troubleshooting:** LM output error.

> ① The detailed error log is as follows:

<br>

```python
I0602 17:19:07.818754 140658323572544 alphageometry.py:565] LM output (score=-22.635742): "99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99"
I0602 17:19:07.819215 140658323572544 alphageometry.py:566] Translation: "ERROR: must end with ;"

I0602 17:19:07.819299 140658323572544 alphageometry.py:565] LM output (score=-27.353218): "99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99 99********************************************************************************************************************************************************************************************************************************"
I0602 17:19:07.819357 140658323572544 alphageometry.py:566] Translation: "ERROR: must end with ;"

I0602 17:19:07.819427 140658323572544 alphageometry.py:539] Depth 1. There are 0 nodes to expand:
```

<br>

> ② It is suspected to be due to package conflicts, resulting in the alphageometry mode not functioning properly.

> ③ In relation to this, it might be worth trying the installation method using the following [run.sh](https://blog.kakaocdn.net/dn/bvifig/btsITupiwRM/kkBeS5szIKKVLIJZhMKO5k/run.sh?attach=1&knm=tfile.sh), referencing [salihmarangoz's commit](https://github.com/google-deepmind/alphageometry/pull/54/commits/3debb824a1c199c789fb8d4f562dc1319ac9c6c8) (initially, an error occurs).

<br>

<br>

## 3. Syntax

⑴ Definition of points, lines, and planes

> ① `d = free`

>> ○ `y = free y`

> ② `a b = segment a b`: AB is a line segment

>> ○ `b c = segment`: BC is a line segment

> ③ `a b c = triangle`: ABC is a triangle

> ④ `d = eq_triangle d a b`: DAB is an equilateral triangle

> ⑤ `s c p = iso_triangle s c p`: SCP is an isosceles triangle with SC and SP being equal in length

> ⑥ `c a b = r_triangle c a b`: CAB is a right triangle with ∠C being a right angle

> ⑦ `c a b = risos c a b`: Given a right triangle ABC where AB is equal to AC and AB is perpendicular to AC

> ⑧ `a b c d = quadrangle a b c d`: ABCD is a quadrangle

> ⑨ `d e = square a c d e`: DE is two points of the square ACDE

> ⑩ `a b c d = rectangle a b c d`: ABCD is a rectangle 

> ⑪ `a b c d = trapezoid a b c d`: ABCD is a trapezoid with AB and CD parallel to each other.

> ⑫ `a b c d = eq_trapezoid a b c d`: ABCD is an isosceles trapezoid with AB and CD parallel, and AD equal to BC. 

> ⑬ `a b c d = isquare a b c d`

> ⑭ `c = nsquare c b a`

> ⑮ `d = psquare d a b`

> ⑯ `a b c d e = pentagon a b c d e`: ABCDE is a pentagon 

⑵ Definition of special points

> ① `o = midpoint o b c`: O is the midpoint of segment BC

> ② `o = circle o a b c`: O is the circumcenter of triangle ABC

>> ○ `o1 = circle a d c`: O1 is the circumcenter of triangle ADC

>> ○ `o = circumcenter o b i c`: O is the circumcenter of triangle BIC 

> ③ `i = incenter i a b c`: I is the incenter of triangle ABC

> ④ `t1 t2 t3 i = incenter2 t1 t2 t3 i a b c`: The incircle I of triangle ABC touches BC, CA, and AB at T1, T2, and T3, respectively

> ⑤ `h = orthocenter h a b c`: H is the orthocenter of triangle ABC

> ⑥ `t = mirror t r s`: S is the midpoint of RT

> ⑦ `x1 = reflect x1 h1 t1 t2`: X1 is the reflection of H1 over line segment T1T2

> ⑧ `w@6.9090049230038776_-1.3884003936987552`: W is the point at (6.9090049230038776, -1.3884003936987552)

> ⑨ `m l k j = excenter2 m l k j a b c`: J is the excenter of triangle ABC, and M, L, K are the feet of the perpendiculars from J to BC, AC, and AB, respectively

⑶ A point is on a specific line or circle

> ① `a = on_line a b q`: A, B, and Q are collinear (B does not necessarily lie between A and Q)

>> ○ `a1 = on_line b c`: A1 is on the line segment BC

> ② `m = on_circle m g1 a`: M is on the circle G1 passing through A

> ③ `z = on_aline z a p a b x`: ∠ZAP = ∠ABX

>> ○ `e = on_aline d a d c b`: ∠EDA = ∠DCB

> ④ `o = on_bline o r s`: O is on the perpendicular bisector of RS

>> ○ `x = on_bline b c`: X is on the perpendicular bisector of BC

> ⑤ `c = on_pline c m a b`: Line segment CM is parallel to AB

>> ○ `q = on_pline p a b`: Line segment PQ is parallel to AB

> ⑥ `q = on_tline q m w m`: Line segments QM and WM are perpendicular or Q is on the circle tangent to WM at point M

>> ○ ` h = on_tline b a c`: The line segment HB is perpendicular to AC, or H is the point where a circle tangent to line segment AC passes through point B.

> ⑦ `h1 = foot h1 a b c`: H1 is the foot of the perpendicular from A to BC

> ⑧ `q = on_dia q a h`: AQ is perpendicular to HQ

> ⑨ `d = eqdistance d a b c`: AD = BC

>> ○ `e = eqdistance d b c`: ED = BC

> ⑩ `x = parallelogram e a m x`: X is the remaining vertex of parallelogram EAMX

> ⑪ `q t p s = cc_tangent q t p s i1 f1 i2 f2`: When there are circles centered at I1 passing through F1 and at I2 passing through F2, the common tangents of the two circles are QT and PS

> ⑫ `q = intersection_cc q a o p`

> ⑬ `f = intersection_lc f e d b`

> ⑭ `f = intersection_ll f b d c e` 

> ⑮ `f = intersection_lp f a c e c b` 

> ⑯ `d = intersection_pp d a b c c a b` 

> ⑰ `d = intersection_lt d c e a a o` 

> ⑱ `e = intersection_tt e b b o c c o` 

> ⑲ `f = shift f c a e`

> ⑳ `r = lc_tangent r p a`

> ㉑ `d x = trisegment d x c b`

> ㉒ `d e = trisect d e b a c`

> ㉓ `d e f = 3peq d e f c a b`

> ㉔ `f g h i = 2l1c f g h i a b e d`

> ㉕ `e g = e5128 e g a b c d`

⑷ A point is at a specific angle

> ① `f = angle_bisector f b a z`: F is on the angle bisector of ∠BAZ

>> ○ `d = angle_bisector b a c`: D is on the angle bisector of ∠BAC

> ② `e = angle_mirror e c a d`: ∠EAD = ∠CAD

> ③ `a = eqangle2 b t e`: ∠ABT = ∠AET

> ④ `p1 = eqangle3 p c a b c`: ∠BAC = ∠P1PC

> ⑤ `c = s_angle b a c 60`

⑸ Definition of problems

> ① `? cong o p o q`: Show that OP = OQ

> ② `? coll x o a`: Show that X is on segment OA

> ③ `? perp k t o1 t`: Show that segment KT is perpendicular to segment O1T

> ④ `? para d e f g`: Show that segment DE is parallel to FG

> ⑤ `? eqangle e c e j e j e f`: Show that ∠CEJ = ∠JEF, i.e., EJ bisects ∠CEF

>> ○ `? eqangle e c c d d f f c`: Show that ∠ECD = ∠DFC 

>> ○ `? eqangle f g f b c f c b`: Show that ∠GFB = ∠FCB 

> ⑥ `? cyclic p q r m:` Show that P, Q, R, and M lie on the same circle (cyclic quadrilateral proof)

> ⑦ `? eqratio k k1 l l1 r q r p`: Show that KK1 / LL1 = RQ / RP (where AB denotes the length of segment AB)

> ⑧ `? midp g a d` : Show that G is the midpoint of AD

> ⑨ `? simtri e f g e c b` : Show that ΔEFG is similar to ΔECB

> ⑩ `? contri e a b a b e`

<br>

---

_Input: 2024.05.05 22:23_
