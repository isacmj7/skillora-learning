---
name: B1 — ML and Data Foundations for Interviews
course_id: B1
track: B (AI Engineer)
version: '1.0'
status: 'APPROVED WITH CONDITIONS — see Human Action Register'
date: 2026-10-05
last_reviewed: 2026-10-06
integration_pass: INT-A 2026-10-05
draft_lineage: wave-2 v2.0 (INT-A 2026-10-05; track lead 2026-10-06)
boss_approved: 2026-10-06
conditions: HAR-01, HAR-12 (see _reviews/Boss_Final_Approval.md)
---

# B1 — ML and Data Foundations for Interviews (Track B, AI Engineer)

Last drafted: 2026-10-05 by SME agent B1. Wave-2 upgrade (v2.0, learner-fit and boss rulings applied): October 2026. Status: see front matter (boss approved 2026-10-06; conditions in the Human Action Register). Version facts in this file are "as of October 2026".

## 1. Course header

| Field | Content |
|---|---|
| Course ID / title / track | B1 — ML and Data Foundations for Interviews / Track B (AI Engineer) |
| Audience | Senior backend engineer (L3–L4 in backend, **L1–L2 in ML and statistics**) targeting AI Engineer and Python+AI hybrid interviews; also serves any L1–L4 learner (the Learner-Fit Appendix, section 8, defines the paths). Not a full ML degree: the goal is to answer the ML-fundamentals round credibly, read a model card or paper abstract without fear, make sound ML-versus-LLM decisions, and explain tokens, embeddings and transformers at interview depth. Includes Simpler scaffolding (maths refreshers, Pandas basics) for gaps. |
| Prerequisites | Working Python and SQL; comfort with functions, lists/dicts, basic statistics vocabulary (mean, variance, probability). No calculus or linear algebra beyond what the course re-teaches (vectors, dot product, matrix multiply, gradient intuition; refreshers are Simpler-path content). Route position (boss dependency graph): B1 is a recommended (dotted) predecessor of B2 and a required (solid) predecessor of B7; the adaptive engine may place it early (weeks 1-3 thread) or defer M7 to just before B7. |
| Placement skip | Per module, by G-5 bands (>= 85 skip lessons and take the mastery check; 70-84 skim; 50-69 study; < 50 deep-study with Simpler path). M6 (NumPy and Pandas) is **standalone-testable** with its own placement items and is the only module whose lessons a strong L3/L4 learner commonly skips outright; A1 and A3 link to it as optional (R-B1-4). M4 (metrics), M5 (leakage) and M9 (transformers) are rarely skipped because they are the most probed. No band waives the final assessment; whole-course test-out requires the final mastery check at 85% plus a Senior-bar voice mock (G-5). |
| Outcomes (each tied to an interview competency) | 1. Frame a business problem as supervised, unsupervised, ranking or generation, and pick a baseline (`ml.framing`). 2. Diagnose under- and over-fitting from learning curves and explain bias-variance, regularisation and cross-validation, including time-series and grouped splits (`ml.generalisation`). 3. Choose and justify evaluation metrics for imbalanced classification, regression and ranking, and set thresholds from business cost (`ml.metrics`). 4. Detect and prevent data leakage and data-quality failures; design features and pipelines that are identical in training and serving (`ml.data-quality`). 5. Write fluent vectorised NumPy and Pandas for realistic data tasks without loops (`data.numpy-pandas`). 6. Explain neural network training (loss, backprop, optimisers, regularisation) and the difference between training and inference cost (`ml.deep-learning-basics`). 7. Explain embeddings, tokenisation and the transformer/attention mechanism at interview depth, including KV cache and context-length cost (`ml.transformers`, `ml.embeddings`). 8. Design an A/B test or offline experiment, and decide when a classical model, an embedding model or an LLM is the right tool (`ml.experiment-design`, `ml.model-choice`). |
| Estimated effort | **19.5 h Simpler / 13 h Deeper**, plus 2.0 h allowance for final assessment, voice mock and revision loops (boss effort table; the statistics lesson in M10 added 1.5 h). Simpler is the L1–L2 route with full scaffolding; Deeper is the L3–L4 route net of typical skip-ahead, so it is shorter in wall-clock hours but harder per hour (G-2). Per-module hours are in the module table and sum to the header. Practice is roughly 40% of the time. Route weight (G-3): Backend `N`, AI Engineer `M`, Hybrid `S`. |
| Interview coverage tags (owned, G-9) | `ml.framing`, `ml.generalisation`, `ml.classical-models`, `ml.metrics`, `ml.data-quality`, `data.numpy-pandas`, `ml.deep-learning-basics`, `ml.embeddings`, `ml.transformers`, `ml.experiment-design`, `ml.model-choice`. Secondary (no credit): `llm.fundamentals` (owner B2; tokenisation mechanics stay in B1.M8). Feeds downstream: B2 (tokens, context), B3 (embeddings concept, retrieval metrics), B5 (eval design, statistics for evals), B7 (training vs inference, KV-cache formula). |
| Badge and certificate | Ten module badges (mastery check at or above 80% plus at least 70% of hidden tests passing on the coding tasks, G-7). Course badge "ML and Data Foundations": all module badges, the final mastery check at or above 80%, the timed data task passed, and one voice mock round at "Meets" (average >= 3.0, none < 2). **Interview-ready** is a separate, stricter state (G-6; section 8, LF-2). Certificate text states the course version and last-reviewed date. |
| Authoring owner | SME agent B1 |
| Reviewer | To be assigned |
| Review cadence | Stable ML theory (M1, M2, M4, M5, M7, M10 statistics): every 12 months. Library-sensitive and AI-sensitive content (M6 library behaviour, M8 tokenisation, M9 and the LLM-comparison parts of M3 and M10): every 3 months. Last reviewed: October 2026. |

### 1.1 Teaching principles for this course

- Interview-first: every concept is taught as "definition in one sentence, intuition, one number or code line that proves it, the trap interviewers set".
- Backend-engineer bridges: map ML ideas onto things the learner already knows (train/serve skew as schema drift between services; leakage as reading from a replica that is ahead of your snapshot; feature pipelines as ETL contracts; embeddings as a lossy index key).
- Everything runs: each lesson has a runnable notebook-style snippet (P-18 cell runner with SVG plots and a precomputed-output fallback) on the **platform interpreter, Python 3.14** (G-8, R-B1-3), with libraries taken from the single platform pin manifest. As of October 2026 the families are: NumPy 2.4 (Dec 2025, Python 3.11+) and 2.5.x (June 2026, requires Python 3.12+; both ship cp314 wheels), pandas 3.0.x (3.0.0 on 2026-01-21), scikit-learn 1.8 (Dec 2025) and 1.9.x (1.9.0 June 2026), PyTorch 2.10 to 2.14 (all ship cp314 wheels; 2.14.x current at 2026-10) (verified 2026-10, PyPI metadata); the free-threaded build is not used in B1 (A1.M3 and A2.M1 only, G-8), so no free-threading claim is made here. Exact patch pins live in the manifest, not here. Limits per P-02: at most 2 minutes per run, 4 GB RAM for PyTorch tasks, CPU only, no GPU. Heavier demos use precomputed outputs. Gradient-boosting libraries (XGBoost, LightGBM, CatBoost) are shown with scikit-learn's `HistGradientBoostingClassifier` as the always-available stand-in; XGBoost 3.4.x (requires Python 3.12+, Python 3.14 classifier), LightGBM 4.7.0 (Python 3.14 classifier) and CatBoost 1.2.10 (cp314 wheels) all install on Python 3.14 (verified 2026-10, PyPI metadata); pins live in the manifest.
- Experience bridges are in every module (section 8, LF-3): you already operate these ideas in production under other names (schema drift, replicas ahead of snapshot, cache invalidation, canary releases); the course names the ML equivalent and the senior-depth gap.
- Maths is kept to what is explained verbally in interviews: dot products, cosine similarity, softmax, the shape of a gradient step, the form of the attention formula.

## 2. Module table

| # | Module | Module outcome | Key topics | Visuals | Practice tasks | Pitfalls and misconceptions interviewers listen for |
|---|---|---|---|---|---|---|
| M1 | ML framing, paradigms and the workflow | Frame a problem as the right ML task, choose a baseline and describe the end-to-end lifecycle in 3 minutes. | Supervised (classification, regression), unsupervised (clustering, dimensionality reduction, anomaly detection), self-supervised (the basis of LLM pre-training), semi-supervised, reinforcement learning at one-paragraph level; ranking and recommendation as their own task type; labels, features, target, train/validation/test roles; baselines (majority class, simple heuristic, linear model); the lifecycle from problem to monitoring; ML versus rules versus LLM at first pass. | Lifecycle flow (problem, data, baseline, model, evaluate, deploy, monitor, retrain); decision tree "which learning paradigm"; role of train/validation/test as a three-bucket diagram. | Classify 12 business scenarios into task types and name a baseline and a metric for each; build a baseline classifier with scikit-learn `DummyClassifier` and compare to logistic regression on a tabular set; write the "problem framing one-pager" for a churn use case. | "Deep learning for everything"; skipping the baseline; confusing unsupervised with self-supervised; treating the test set as a tuning aid; calling any LLM feature "machine learning" without being able to say what is learned from what. |
| M2 | Generalisation: bias-variance, overfitting, regularisation, cross-validation | Diagnose under/over-fitting from curves and choose a remedy; design a validation scheme that matches how the model will be used. | Bias-variance decomposition at intuition level; training versus validation error curves; learning curves (error versus training-set size); model capacity; L1 versus L2 (sparsity versus shrinkage), elastic net, early stopping, dropout (preview), data augmentation, more data; k-fold, stratified, group k-fold, time-series split and why random split fails for temporal data; nested CV and the tuning-on-test trap; hyperparameter search (grid, random, Bayesian at concept level); the bias-variance story for ensembles. | Dartboard bias-variance picture; interactive polynomial-degree slider showing train and validation error; learning-curve diagnoser; CV fold diagram including time-series expanding window. | Fit polynomial models of increasing degree and plot error curves; implement k-fold by hand then compare to `cross_val_score`; use `TimeSeriesSplit` and `GroupKFold` on a dataset with repeated users; tune L1/L2 strength and plot the coefficient path. | "More complex model is always better"; "regularisation just reduces training error" (it trades bias for variance); random split on time-ordered data; scaling the full dataset before splitting; reporting the best CV score as the expected production score after heavy tuning; "high variance means high error" confusion. |
| M3 | Classical models and when to use them over LLMs | Pick among linear/logistic, trees, ensembles, kNN, SVM and clustering with reasons; know when a classical model beats an LLM and when it does not. | Linear regression (assumptions, residuals, multicollinearity), logistic regression (sigmoid, log-odds, why it is still a go-to baseline, coefficients as interpretable effects, class weights), decision trees (splits by impurity, depth control), random forests (bagging, feature randomness, out-of-bag estimate), gradient boosting (XGBoost, LightGBM, CatBoost, sequential error correction, why it dominates tabular data), kNN and the curse of dimensionality, SVM at concept level (margin, kernel idea), k-means (and its limits: spherical clusters, choosing k with elbow and silhouette), hierarchical and DBSCAN, PCA (variance, when to use), Naive Bayes and TF-IDF plus linear model as a strong text baseline; interpretability tools (feature importance, permutation importance, SHAP at concept level); decision guide: structured tabular data, latency and cost constraints, small labelled data, text classification, extraction, open-ended generation. | Model-family comparison matrix (data type, interpretability, latency, tuning effort); bagging versus boosting side-by-side animation; tree split step-through; k-means iteration animation; "classical, embeddings plus classifier, or LLM" decision tree. | Train logistic regression, random forest and gradient boosting on one tabular set and compare metrics, training time and inference latency; build TF-IDF plus logistic regression for ticket classification and compare with a zero-shot LLM prompt on a labelled sample (cost, latency, accuracy table); cluster customer data with k-means and justify k; compute permutation importance and explain one surprising feature. | "Random forest and gradient boosting are the same thing"; "boosting does not overfit"; trees need feature scaling (they do not; linear models and kNN do); using accuracy to compare models on imbalanced data; k-means on unscaled features; "LLMs make classical ML obsolete" (and the opposite, "LLMs are never the answer for classification"); interpreting logistic coefficients without considering scale or correlation. |
| M4 | Evaluation metrics | Choose, compute and defend metrics for classification, regression and ranking, and turn a business cost into a threshold. | Confusion matrix, accuracy and why it misleads under imbalance, precision, recall, F1 and F-beta, specificity, ROC curve and ROC-AUC (probability a random positive scores above a random negative), precision-recall curve and PR-AUC/average precision and why PR is preferable under heavy imbalance, threshold selection from cost matrix, calibration (reliability diagrams, Brier score, why probabilities from boosting may be uncalibrated), multi-class averaging (macro, micro, weighted), regression metrics (MAE, MSE, RMSE, R-squared and its limits, MAPE pitfalls near zero, quantile loss), ranking and retrieval metrics (precision@k, recall@k, MRR, MAP, NDCG), basic text-generation metrics at awareness level (BLEU/ROUGE limits, LLM-as-judge pointer to B5), confidence intervals via bootstrap, offline versus online metrics, metric-business alignment and guardrail metrics. | Interactive threshold slider moving a point on ROC and PR curves and updating confusion matrix; confusion-matrix heatmap builder; NDCG worked table; metric chooser flowchart. | Compute precision, recall, F1 by hand then verify with scikit-learn; plot ROC and PR on a 1% positive-rate dataset and explain why they differ; pick a threshold that minimises expected cost given a cost matrix; implement MRR, recall@k and NDCG@k from scratch in NumPy; bootstrap a 95% confidence interval for F1. | "99% accuracy means a great model" on rare-event data; confusing precision with recall; ROC-AUC treated as insensitive to anything (it is insensitive to class prior, which is exactly why it can hide poor precision); using 0.5 as the threshold by default; averaging F1 across classes without saying which average; R-squared as accuracy; comparing metrics across datasets with different base rates; optimising an offline metric that is not the product metric. |
| M5 | Data quality, leakage and feature engineering | Spot leakage and data-quality problems in a described pipeline and design a leak-free, reproducible feature pipeline. | Types of leakage (target leakage, train-test contamination from preprocessing before split, temporal leakage, group/entity leakage, duplicate rows across splits, label leakage through proxy features); detection (suspiciously high score, feature importance sanity, time-travel check); missing data (MCAR, MAR, MNAR at intuition level; imputation strategies; missing-indicator features); outliers; duplicates; label noise; class imbalance (class weights, resampling, SMOTE caveats, threshold moving, why resample only the training fold); feature scaling and encoding (one-hot, ordinal, target encoding and its leakage risk, hashing, frequency); numeric transforms (log, binning), date/time features, text features (TF-IDF), interaction features; feature selection; scikit-learn `Pipeline` and `ColumnTransformer` to guarantee the same transform in train and serve; train-serving skew and feature stores at concept level; data drift versus concept drift and basic monitoring (PSI, KS test at awareness level); data contracts and validation (schema checks, Great Expectations or Pandera style; tool landscape verified 2026-10: GX Core 1.x, Pandera 0.33 with a Python 3.14 classifier, Evidently 0.7.x; examples only, `[verify: GX on Python 3.14]` because GX docs list 3.14 but PyPI metadata for great-expectations 1.23.2 declares Python <3.14; exact check: `pip install great-expectations` and a minimal validation run on the platform 3.14 image in CI, owner platform engineering; if it fails, GX appears only as read-only example code and Pandera is the runnable validation example); dataset versioning and lineage. | Leakage "time-travel" timeline; pipeline diagram showing fit-on-train-only; train-serve skew diagram with two code paths; drift types quadrant. | Given three flawed notebooks, find the leak in each and fix it; build a `Pipeline` with `ColumnTransformer` that handles numeric, categorical and text columns and serialise it for reuse; implement leak-free target encoding with out-of-fold statistics; write validation checks (schema, ranges, null rates, duplicates) for an incoming batch; compute PSI between training and a shifted batch. | "Scale first, then split"; "oversample then cross-validate" (leaks synthetic neighbours into validation); target encoding with full-dataset means; using post-event fields as features; assuming drift means the model is wrong without checking the metric; treating missing values as always random; feature logic written twice (SQL for training, Python for serving). |
| M6 | NumPy and Pandas fluency | Perform realistic data-wrangling tasks with vectorised NumPy and idiomatic Pandas, and explain performance and correctness trade-offs. | NumPy: ndarray, dtype, shape, strides and views versus copies, broadcasting rules, vectorisation versus Python loops, boolean masks, fancy indexing, `axis` semantics, `argsort`, `argpartition`, `einsum` at awareness level, random generators (`default_rng`), numerical stability (log-sum-exp, float precision), matrix multiply and cosine similarity at scale. Pandas: Series/DataFrame, index alignment, `loc` versus `iloc`, boolean filtering, `groupby` with `agg`, `transform` and `apply` (and why apply is slow), `merge` and join types with cardinality checks, `concat`, `pivot_table`, `melt`, `crosstab`, window functions (`rolling`, `expanding`, `shift`, `rank`), datetime handling and `resample`, categorical dtype, memory reduction (downcasting, categories), method chaining, `assign`, `query`, missing values (`isna`, `fillna`, nullable dtypes), copy-on-write semantics (as of October 2026, pandas 3.0 made copy-on-write the default and only mode: indexing results behave as copies, chained assignment no longer modifies the parent, `SettingWithCopyWarning` is gone, and strings default to a dedicated `str` dtype; legacy 2.x behaviour is taught as a "reading old code" note), chunked reading of large files, when to move to SQL, Polars or DuckDB (awareness only). SQL-to-Pandas mapping for analysts. | Broadcasting shape-alignment animation; view-versus-copy diagram; groupby split-apply-combine diagram; join-type Venn plus cardinality-explosion illustration; memory-profile bar chart before and after dtype tuning. | 8 wrangling katas with hidden tests (the four coding-bank items are drawn from them; top-N per group, rolling averages per user, sessionisation by time gap, dedupe keep-latest, pivot and unpivot, cohort retention table, anti-join to find missing rows, outlier clipping); vectorise a loop-based cosine similarity for 100k vectors and measure speedup; reduce a 1 GB-class frame's memory by half with dtypes and prove the results are unchanged; translate five SQL queries to Pandas and back. | Using `iterrows` or `apply` where a vectorised operation exists; chained indexing assignment; accidental many-to-many merge duplicating rows; confusing views and copies; forgetting index alignment on arithmetic between Series; `groupby` silently dropping NaN keys (depends on `dropna` parameter); mutating inputs; float equality checks; reading entire large files into memory. |
| M7 | Neural network basics and training mechanics | Explain how a neural network learns and why training differs from inference, and diagnose common training problems. | Neuron, layer, activation (ReLU, GELU, sigmoid, tanh, softmax) and why non-linearity is required; forward pass as matrix multiplications; loss functions (MSE, cross-entropy) and their link to probabilities; gradient descent, learning rate, batch, epoch, mini-batch SGD; backpropagation as the chain rule applied efficiently (intuition, not derivation); optimisers (SGD with momentum, Adam/AdamW, learning-rate schedules and warm-up); initialisation, vanishing and exploding gradients, normalisation (batch norm versus layer norm), residual connections; regularisation in networks (weight decay, dropout, early stopping, data augmentation); overfitting a small batch as a debugging step; CNN and RNN/LSTM in one paragraph each and why transformers displaced RNNs for language; transfer learning and fine-tuning as **one conceptual paragraph (at most 15 minutes) with a pointer to B7** (all fine-tuning mechanics, GPU memory arithmetic and calculators are B7, R-B1-1); training versus inference (forward-only, no gradients, batching; training memory as a list of components: parameters, gradients, optimiser states, activations, with the optimiser-state intuition conceptual only and **no numeric multipliers taught as facts** because they depend on precision and optimiser; the paper calculation uses learner-stated assumptions), mixed precision, GPU versus CPU at concept level, the compute cost intuition (parameters times tokens). | Computation-graph step-through of a tiny two-layer network with numbers; loss-landscape and learning-rate too-small/right/too-large animation; training-loop flowchart; memory stack diagram (weights, gradients, optimiser state, activations). | Implement a two-layer network with NumPy forward and backward pass on a toy dataset and verify the gradient numerically; train a small PyTorch MLP on a tabular set and plot loss curves under three learning rates; deliberately cause overfitting then fix with weight decay and early stopping; list the memory components of training versus serving a model and reason qualitatively about which dominate (calculators and worked GPU arithmetic are B7.M8). **Applied lab:** diagnose three recorded training logs from a churn model (flat loss, NaN loss, diverging validation loss) and write the fix for each. | "Backprop is a learning algorithm" (it computes gradients; the optimiser updates weights); "deep learning always needs big data" (transfer learning); confusing epoch, batch and iteration; saying dropout is used at inference; "Adam always beats SGD"; ignoring that training memory is dominated by more than the weights; thinking inference and training cost are the same shape. |
| M8 | Embeddings and tokenisation | Explain what embeddings represent, how similarity is measured and how text becomes tokens, and reason about the cost and quality consequences. | What an embedding is (dense vector learned so that useful similarity is geometric), one-hot versus dense, word embeddings (word2vec idea), contextual embeddings from transformers, sentence and document embedding models, dimensionality, normalisation; similarity metrics (cosine, dot product, Euclidean) and when they coincide (unit-normalised vectors); how embedding models are trained (contrastive learning at concept level, positives and negatives); embeddings for items, users and images (two-tower recommenders at concept level), multimodal embeddings at awareness level; limits (domain shift, negation and numbers, multilingual gaps, anisotropy, versioning: changing models forces re-embedding); exact versus approximate nearest neighbour search as **one slide on the recall-latency-memory trade-off** (index internals such as HNSW and IVF are B3.M4; embedding-model selection, dimensions, Matryoshka, quantisation and query/document prompts are B3.M3; token accounting in application cost is B2.M1); dimensionality reduction for visualisation (PCA, t-SNE, UMAP caveats). Tokenisation: why subwords, BPE, WordPiece and SentencePiece at concept level, byte-level fallbacks, vocabulary size trade-off, special tokens, chat templates, why token counts differ by model and language (Indic scripts and code typically cost more tokens per unit of meaning; the ratio is **generated live in the lab with the bundled tokeniser, never hard-coded**), whitespace and number handling quirks, effects on cost, context limits and some failure modes (spelling, arithmetic, rare words), counting tokens before sending. | Embedding-space 2D projection with clusters and a query arrow; cosine-similarity geometry diagram; BPE merge step-through on a short corpus; token-count comparison chart for English versus Hindi versus code; two-tower recommender sketch. | Compute embeddings for 200 short texts using a small local model or provided precomputed vectors, then implement top-k cosine search in NumPy and evaluate recall@k against labels; show that normalised dot product equals cosine; implement a toy BPE tokeniser on a small corpus; **applied lab:** build a token-budget checker for a FastAPI chat endpoint that truncates history by tokens, not characters; compare token counts of the same content in three languages and compute cost per 1,000 requests with stated prices as parameters (no real prices hard-coded); visualise clusters with PCA and explain what the plot cannot prove. | "Embeddings capture meaning perfectly"; mixing embeddings from different models in one index; using Euclidean on unnormalised vectors and assuming cosine behaviour; tokens equal words or characters; assuming 1 token is about 0.75 words for every language; ignoring that tokenisation affects prices and context fit; trusting t-SNE distances and cluster sizes. |
| M9 | Transformers, attention, training versus inference at interview depth | Explain the transformer end to end, derive the cost of attention, and explain pre-training, fine-tuning, decoding, KV cache and context-window economics. | Why attention (long-range dependencies, parallel training versus RNNs); input path: tokens to embeddings plus positional information (sinusoidal, learned, rotary at concept level); self-attention: queries, keys, values, scaled dot-product, softmax, weighted sum, why divide by square root of d_k; multi-head attention and what heads buy; causal masking (decoder-only) versus bidirectional (encoder); the transformer block (attention, feed-forward, residuals, layer norm, pre-norm); encoder-only (BERT-style, embeddings, classification), decoder-only (GPT-style, generation) and encoder-decoder (translation, T5-style) and when each is used; attention cost O(n^2) in sequence length and memory consequences, FlashAttention, grouped-query/multi-query attention, sliding-window attention and latent-KV compression (multi-head latent attention) at awareness level, long-context trade-offs and the "lost in the middle" caveat (taught as a hedged, evolving finding: positional position effects on retrieval of facts from long prompts were reported in published studies and vary by model; learners are told to test their own model, not to quote the effect as a law); pre-training objective (next-token prediction), post-training (supervised fine-tuning, preference tuning such as RLHF/DPO and reinforcement learning against verifiable rewards as the basis of reasoning-model post-training, all at concept level; depth in B7, effect on tokens and cost in B2.M2); decoding (greedy, beam, temperature, top-k, top-p, repetition penalties), why sampling makes outputs non-deterministic and why temperature 0 is still not guaranteed bit-exact across hardware or batches; inference phases: prefill (compute-bound) versus decode (memory-bandwidth-bound), KV cache with the **full size-formula derivation owned here** (bytes = 2 for K and V x layers x KV heads x head dimension x sequence length x batch size x bytes per element; B7.M7-M8 reuse it without re-deriving), its memory growth, batching and continuous batching, quantisation, speculative decoding at awareness level, time-to-first-token versus tokens-per-second; mixture-of-experts at one-paragraph level; scaling-law intuition (more parameters, data and compute help with diminishing returns) without quoting unverifiable numbers; why hallucination arises (the model optimises plausible continuation, not truth). | Attention matrix heat-map for a short sentence; step-through of Q, K, V computation with tiny 3-token example; transformer block diagram (valid Mermaid); KV cache growth timeline; prefill versus decode timeline; encoder versus decoder masking diagram. | Implement scaled dot-product attention with causal mask in NumPy on a toy example and check shapes; add multi-head splitting; calculate KV-cache memory for a given layers, heads, head-dim, sequence length and batch size (parameters supplied) and discuss the effect of grouped-query attention; run temperature, top-k and top-p on a toy logit vector and plot distribution changes; explain via a written exercise why doubling context length does not double latency but can quadruple attention compute. | "Attention is just a lookup"; forgetting the causal mask or confusing encoder and decoder behaviour; saying the model "understands" without caveats; stating context windows are free; believing temperature 0 is fully deterministic; saying training and inference both use backprop; "the LLM remembers earlier chats" (state is in the context you resend); mixing up parameters, tokens and context length when estimating cost. |
| M10 | Experiment design, A/B testing and the ML-versus-LLM decision | Design a sound offline and online experiment for an ML or LLM feature, interpret results, and recommend the right model class with cost and risk reasoning. | Offline versus online evaluation; hypothesis, primary and guardrail metrics, unit of randomisation, sample size and power at intuition level (minimum detectable effect, significance level, power), p-values and confidence intervals read correctly, peeking and sequential testing problem, novelty and seasonality effects, SRM (sample-ratio mismatch) check, multiple comparisons, network and interference effects at awareness level, CUPED variance reduction awareness, A/A tests, holdouts and long-term effects, shadow mode, canary and interleaving for ranking; counterfactual and offline-policy evaluation as awareness only; experiment logging and reproducibility (seeds, data versions, config capture, MLflow or Weights and Biases style tracking; landscape verified 2026-10: MLflow 3.x active, Weights and Biases now part of CoreWeave since May 2025; examples only); human evaluation basics and inter-annotator agreement (Cohen's kappa); evaluating LLM features and pointer to B5 (methodology owner); **statistics-for-evals lesson (1.5 h, owned here, used by B5):** sampling variability and standard error, bootstrap confidence intervals for any metric (F1, recall@k, pass rate), reading p-values correctly (what they are and are not), confidence interval versus significance, power and minimum detectable effect, multiple comparisons (Bonferroni and false-discovery rate at concept level), why a 50-item eval set cannot separate two models 2 points apart, paired versus unpaired comparison on one eval set; ML-versus-LLM decision framework: data type, label availability, latency and cost per call, accuracy requirement, explainability, privacy, volume, change frequency, maintenance burden; hybrid designs (LLM for cold start labels, distilled classifier for volume; embeddings plus classifier; LLM as router or reranker over classical retrieval); build-versus-buy and total cost reasoning; designing an ML feature end to end (data, labels, features, model, serving, monitoring, retraining trigger, fallback). | A/B test timeline with peeking trap; power-analysis curve (sample size versus MDE); SRM check illustration; decision flow "rules, classical, embeddings plus classifier, fine-tuned small model, prompted LLM"; ML feature reference architecture (valid Mermaid) on the design canvas. | Compute required sample size for a conversion test using a supplied formula and a simulation; simulate peeking and show inflated false-positive rate; run an SRM check with a chi-square test on given counts; write an experiment plan for a "smart reply" feature; produce a one-page cost and accuracy comparison (classical versus LLM versus hybrid) for a ticket-routing use case with explicit assumptions. | Stopping a test when it first looks significant; no guardrail metrics; randomising by request when the effect is per user; treating "not significant" as "no effect"; ignoring SRM; using an LLM when a regex or a gradient-boosted model is cheaper, faster and auditable; the reverse, forcing a classical pipeline on messy unstructured text with no labels; ignoring monitoring and rollback in ML designs. |

| | **Ownership notes (G-1)** | | **OWNS:** ML framing, generalisation, classical models, metrics, leakage and train-serve skew (M1-M5); NumPy/Pandas fluency (M6, standalone); neural-net training mechanics and optimiser-memory intuition (M7); embeddings concept, similarity, normalisation, tokenisation mechanics (M8); transformers, attention, KV-cache size formula, decoding (M9); experiment design, A/B testing, statistics for evals, ML-versus-LLM decision (M10). **POINTS:** fine-tuning mechanics and GPU memory arithmetic -> B7.M2-M5, B7.M8 (M7, 15 min); embedding-model selection -> B3.M3; ANN indexing (HNSW, IVF) and pgvector -> B3.M4 (M8, one slide); token accounting in app cost, caching -> B2.M1, B2.M9 (M8); LLM-as-judge and eval methodology -> B5.M1-M3 (M4, M10); retrieval metric definitions for RAG -> B3.M8 (M4 keeps ranking metrics generally); prompt vs RAG vs fine-tune -> B7.M1 (M10 keeps ML-vs-LLM only); registry and drift for model artefacts -> B7.M9 (M5 keeps data drift awareness). | | | |

### 2.1 Hours per module (sum to header: 19.5 h Simpler / 13 h Deeper)

| Module | M1 | M2 | M3 | M4 | M5 | M6 | M7 | M8 | M9 | M10 | Total |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Simpler (h) | 1.25 | 2.0 | 2.0 | 2.0 | 2.0 | 2.0 | 1.75 | 1.25 | 2.25 | 3.0 | **19.5** |
| Deeper (h) | 0.75 | 1.25 | 1.5 | 1.75 | 1.25 | 0.75 | 0.75 | 0.75 | 1.5 | 2.75 | **13.0** |

M10 includes the 1.5 h statistics lesson on both paths. Deeper hours are net of typical skip-ahead for an L3/L4 backend engineer who is L1-L2 in ML; a learner who scores in a higher band spends less (LF-2). Plus 2.0 h assessment allowance (section 5).

## 3. Per-module detail blocks

Question-bank targets per module (from the authoring standard): 12 MCQ/concept checks, 6 code-output or predict-the-result items, 4 coding tasks with hidden tests, 8 verbal questions with follow-up trees (3 of which are seeded in section 4), 3 scenario/design questions. Mastery check: 8 items, pass at 80% (G-7), revision loop on misses. Placement diagnostics (6 per module, 8 for the standalone M6, LF-2) are tagged `placement` items drawn from the MCQ and predict buckets, not extra authoring. Each lesson carries at least one visual with alt text: the module table names the signature visuals, and the remaining per-lesson diagrams are authored from the lesson lists below.

### 3.1 Worked examples (template section 2, one annotated example per module)

| Module | Annotated worked example |
|---|---|
| M1 | Churn framing one-pager: target, prediction point, action, cost matrix, baseline, split, annotated line by line |
| M2 | Polynomial fit with train/validation curves and a `TimeSeriesSplit` rescue of a leaky random split |
| M3 | One tabular set, three models, one comparison table (metric, training time, p95 inference latency) with commentary |
| M4 | 1% positive-rate set: confusion matrix by hand, PR versus ROC, cost-based threshold, bootstrap CI |
| M5 | A leaky notebook rewritten as a `Pipeline` with out-of-fold target encoding, each fix annotated |
| M6 | Sessionisation and per-user rolling counts: loop version, vectorised version, benchmark, hidden-test checks |
| M7 | Two-layer network, forward and backward by hand with numbers, then NumPy code and a numerical gradient check |
| M8 | Normalised dot product equals cosine; top-k search over 200 texts; toy BPE merges on a ten-word corpus |
| M9 | Three-token scaled dot-product attention with causal mask, then KV-cache bytes for a supplied config with and without grouped-query attention |
| M10 | Sample-size calculation plus a peeking simulation, followed by a paired bootstrap CI on an eval set |

Module badge: mastery check passed plus at least 3 of 4 coding tasks at 70% or more of hidden tests with correct stated complexity. IDs follow `Q-B1-M<n>-<nn>` (G-10).

### M1 — ML framing, paradigms and the workflow
- Lessons: (1) What ML is and is not: rules versus learned functions; (2) Task types and paradigms, including self-supervised and ranking; (3) Data roles and the baseline habit; (4) The lifecycle from problem to monitoring.
- Simpler: plain-language definitions, 12-scenario sorting game, no maths. Deeper: framing trade-offs (proxy labels, delayed labels, feedback loops), cold-start and label-cost thinking.
- Mastery-check focus: task-type identification; baseline choice; train/validation/test roles; lifecycle ordering.
- Badge: "ML Framer". Targets: 12 MCQ / 6 code-output / 4 coding / 8 verbal / 3 scenario.

### M2 — Generalisation: bias-variance, overfitting, regularisation, cross-validation
- Lessons: (1) Bias-variance intuition and error curves; (2) Regularisation family (L1, L2, early stopping, more data); (3) Cross-validation variants and when each is valid; (4) Hyperparameter search and the tuning-on-test trap; (5) Learning curves as a diagnostic tool.
- Simpler: polynomial slider, plain-language remedies table. Deeper: bias-variance decomposition formula, nested CV, why ensembles reduce variance.
- Mastery-check focus: reading curves; picking a split scheme for a given data shape; L1 versus L2 effects.
- Badge: "Generalisation Guard". Targets: standard.

### M3 — Classical models and when to use them over LLMs
- Lessons: (1) Linear and logistic regression done properly; (2) Trees, forests, boosting; (3) kNN, SVM, Naive Bayes, text baselines; (4) Clustering and PCA; (5) Choosing: classical versus embeddings versus LLM.
- Simpler: one model per sitting with a single dataset and one diagram. Deeper: boosting mechanics, regularisation parameters in boosted trees, SHAP interpretation caveats, cost-latency-accuracy tables.
- Mastery-check focus: bagging versus boosting; scaling requirements per model; k-means limits; the decision tree for ML versus LLM.
- Badge: "Model Chooser". Targets: standard.

### M4 — Evaluation metrics
- Lessons: (1) Confusion matrix and the precision-recall family; (2) ROC, PR curves and AUCs; (3) Thresholds, costs and calibration; (4) Regression metrics; (5) Ranking and retrieval metrics.
- Simpler: threshold slider with worked numbers. Deeper: AUC probabilistic interpretation, calibration curves, bootstrap confidence intervals, metric gaming.
- Mastery-check focus: metric choice for rare events; computing P/R/F1 by hand; NDCG on a small list; threshold from cost.
- Badge: "Metrics Master". Targets: standard.

### M5 — Data quality, leakage and feature engineering
- Lessons: (1) The leakage zoo and how to catch it; (2) Missing data, outliers, duplicates, noisy labels; (3) Encoding, scaling and feature construction; (4) Pipelines and train-serve parity; (5) Drift and data validation.
- Simpler: three leak spotter exercises, `Pipeline` basics. Deeper: out-of-fold target encoding, group leakage, PSI/KS monitoring, data contracts.
- Mastery-check focus: identify leak from a description; imbalance handling inside CV; pipeline construction; drift types.
- Badge: "Leak Hunter". Targets: standard.

### M6 — NumPy and Pandas fluency
- Lessons: (1) NumPy arrays, broadcasting and vectorisation; (2) Pandas selection, groupby and window operations; (3) Joins, reshaping and time series; (4) Performance and memory; (5) SQL-to-Pandas translation and when to leave Pandas.
- Simpler: guided katas with hints; Deeper: strides, copy semantics, `argpartition` top-k, chunked processing, benchmarking.
- Mastery-check focus: broadcasting shapes; groupby transform versus agg; merge cardinality; view versus copy.
- Badge: "Data Wrangler". Targets: standard, with coding tasks weighted towards realistic wrangling and timed 20-minute versions.

### M7 — Neural network basics and training mechanics
- Lessons: M7.1 Neurons, layers, activations; M7.2 Loss, gradient descent and backprop intuition; M7.3 Optimisers, schedules, normalisation, regularisation; M7.4 Debugging training; M7.5 Training versus inference: compute and memory components, plus the fine-tuning paragraph (at most 15 min, pointer to B7).
- Simpler: numeric walk-through of one backward pass, with a gradient and chain-rule refresher; Deeper: AdamW, warm-up, mixed precision, qualitative memory components (arithmetic is B7.M8).
- Mastery-check focus: epoch/batch/iteration; role of activations; what backprop does; training memory components.
- Badge: "Training Mechanics". Targets: standard.

### M8 — Embeddings and tokenisation
- Lessons: (1) What embeddings are and how they are trained; (2) Similarity metrics and normalisation; (3) Using embeddings: search, clustering, classification features, recommenders, plus the one-slide recall-latency-memory trade-off (pointer to B3.M3 and B3.M4); (4) Tokenisation and BPE; (5) Cost and failure modes tied to tokens.
- Simpler: nearest-neighbour demo and token counter. Deeper: contrastive training, anisotropy, multilingual token cost generated live.
- Mastery-check focus: cosine versus dot versus Euclidean; why tokens differ across models; embedding versioning and re-embedding cost.
- Badge: "Embedding and Token Fluent". Targets: standard.

### M9 — Transformers, attention, training versus inference
- Lessons: (1) From RNNs to attention; (2) Self-attention, multi-head, masking, the block; (3) Architectures: encoder, decoder, encoder-decoder; (4) Pre-training, post-training and decoding strategies; (5) Inference economics: prefill, decode, KV cache and the size-formula derivation (owner, reused by B7), context cost.
- Simpler: 3-token attention walk-through, no matrix notation beyond shapes. Deeper: positional encodings, GQA effect on the KV formula, FlashAttention intuition, MoE, speculative decoding.
- Mastery-check focus: roles of Q, K, V; why scaling by root d_k; causal mask; cost scaling with sequence length; KV cache; sampling parameters.
- Badge: "Transformer Interview-Ready". Targets: standard.

### M10 — Experiment design, A/B testing and ML-versus-LLM decision
- Lessons: (1) Offline versus online evaluation; (2) A/B test design, power and pitfalls; (3) Ranking and LLM-feature experiments, guardrails and rollout patterns; (4) Experiment tracking and reproducibility; (5) The ML-versus-LLM decision framework, hybrid designs and designing an ML feature end to end; (6) **Statistics for evals (1.5 h; bootstrap confidence intervals, p-value intuition, power, multiple comparisons; used by B5)**.
- Simpler: guided experiment plan template, statistics lesson with a probability refresher. Deeper: sequential testing, CUPED, interleaving, cost modelling with assumptions, paired bootstrap on an eval set.
- Mastery-check focus: sample-size intuition; peeking; SRM; guardrails; bootstrap CI reading; multiple comparisons; model-class selection with justification.
- Badge: "Experiment and Decision Designer". Targets: standard.

## 4. Seed interview questions

Format per item: ideal answer outline, common weak answers, follow-up probes, difficulty, competency tag, company-style cluster. These are style-of-question items, not questions attributed to any company.

### M1 — ML framing, paradigms and the workflow

**Q1.1 "A product manager says 'let's use AI to reduce customer churn.' Walk me through how you would frame and start this."**
- Ideal answer outline:
  - Clarify the decision: what action will the prediction trigger (retention offer, call), who acts, and the cost of false positives versus false negatives.
  - Define the target precisely (churn within 30 days of an observation date) and the prediction point to avoid leakage.
  - Identify data: usage, billing, support tickets; label availability and delay.
  - Establish baselines: simple rule (no login in 14 days), then logistic regression, then gradient boosting.
  - Choose metrics tied to business: precision@k for a limited outreach budget, uplift awareness (predicting who is persuadable differs from who will churn).
  - Split by time, not randomly; plan monitoring and a retraining trigger.
  - Say when ML is not warranted (rule solves 80%) and state a rollout plan (A/B test with holdout).
- Common weak answers: jumps to "train a deep neural network"; no target definition; accuracy as the metric; no mention of the action or the cost; random split.
- Follow-ups: How do you handle that only 3% of customers churn? How would you know the retention offer works rather than the model? What changes if labels arrive after 90 days?
- Difficulty: medium. Tag: `ml.framing`. Cluster: global-product / AI-startup.

**Q1.2 "Explain supervised, unsupervised and self-supervised learning and say where an LLM sits."**
- Ideal answer outline:
  - Supervised: labelled input-output pairs; classification and regression; example.
  - Unsupervised: structure without labels; clustering, dimensionality reduction, anomaly detection.
  - Self-supervised: labels derived from the data itself (next token, masked token); no human labelling at scale.
  - LLMs: pre-trained self-supervised on next-token prediction, then post-trained with supervised examples and preference signals.
  - Practical implication: pre-training yields general representations; downstream tasks need far less labelled data.
  - Mention that RL appears via preference optimisation without oversimplifying.
- Common weak answers: "unsupervised means no human involved"; "LLMs are unsupervised"; no examples.
- Follow-ups: Is clustering evaluated the same way as classification? Where does labelling cost enter an LLM pipeline? What is semi-supervised learning good for?
- Difficulty: easy. Tag: `ml.framing`. Cluster: IT-services / Big4.

**Q1.3 "Why do you always build a baseline first, and what would a good one be for a text classification task?"**
- Ideal answer outline:
  - A baseline sets the floor and exposes data issues and metric bugs cheaply.
  - Types: majority class, heuristic or keyword rules, TF-IDF plus logistic regression, small embeddings plus linear model.
  - It turns "is the complex model worth it" into a measurable gain versus cost and latency.
  - Compare on same split and metric; track inference cost.
  - Strong baselines are often within a few points of complex models; justify complexity by lift.
  - Mention a zero-shot LLM as a baseline too, with cost caveats.
- Common weak answers: "baseline is whatever the old model was"; skipping it because "we have deep learning"; comparing on different splits.
- Follow-ups: How much lift justifies a more complex model? How do you baseline when there are no labels? What if the baseline is already good enough?
- Difficulty: easy to medium. Tag: `ml.framing`. Cluster: AI-startup / Indian-product.

### M2 — Generalisation

**Q2.1 "Explain the bias-variance trade-off and how you would diagnose which problem you have."**
- Ideal answer outline:
  - Bias: error from overly simple assumptions (underfitting); variance: sensitivity to the training sample (overfitting); noise is irreducible.
  - Diagnosis: training and validation error. High both and close implies high bias; low training and high validation implies high variance.
  - Learning curves: if validation error plateaus high with more data, bias; a persistent gap that shrinks with more data, variance.
  - Fixes for bias: richer model, better features, less regularisation. Fixes for variance: more data, regularisation, simpler model, bagging, early stopping.
  - Note the trade-off is not a strict law in modern deep models (double descent as awareness), but the diagnostic habit still works.
- Common weak answers: recites definitions only; "variance means spread of data"; suggests more data for high bias.
- Follow-ups: What does a random forest do to bias and variance? How does regularisation strength move you along the curve? How would learning curves differ if labels are noisy?
- Difficulty: medium. Tag: `ml.generalisation`. Cluster: global-product / Indian-product.

**Q2.2 "Your model scores 0.95 in cross-validation but 0.78 in production. What could explain it?"**
- Ideal answer outline:
  - Leakage in CV (preprocessing before split, duplicates, entity or time leakage).
  - Distribution shift between training data and live traffic (covariate shift, concept drift, seasonality).
  - Train-serve skew (different feature code paths, missing features, default handling).
  - Over-tuning on the CV folds; selection bias.
  - Label differences (production labels delayed or defined differently); metric measured on different populations.
  - Investigation steps: replay production inputs through the offline pipeline, compare feature distributions, time-based holdout, check duplicates.
  - Mitigation: time-based validation, shared feature code, monitoring.
- Common weak answers: "overfitting" only; "retrain with more data" without diagnosis.
- Follow-ups: How do you detect train-serve skew in practice? What validation scheme would you use for this problem? How would you monitor this after the fix?
- Difficulty: medium to hard. Tag: `ml.generalisation`, `ml.data-quality`. Cluster: global-product / AI-startup.

**Q2.3 "L1 versus L2 regularisation: what changes in the model, and when would you pick each?"**
- Ideal answer outline:
  - Both add a penalty on weights to the loss; L2 penalises squared magnitude (shrinks weights smoothly), L1 penalises absolute value (drives some weights exactly to zero).
  - L1 yields sparse models and implicit feature selection; L2 handles correlated features more stably; elastic net combines.
  - Geometric picture: diamond versus circle constraint region.
  - Features must be scaled for the penalty to be fair.
  - Regularisation strength is a hyperparameter chosen by cross-validation.
  - Bayesian view as awareness (priors); in neural nets, L2 appears as weight decay (AdamW decouples it).
- Common weak answers: "L1 is better" without context; forgets scaling; confuses with dropout.
- Follow-ups: What happens to L1 with highly correlated features? Why does the intercept usually go unpenalised? What does large C mean in scikit-learn's logistic regression?
- Difficulty: medium. Tag: `ml.generalisation`. Cluster: IT-services / global-IT-consulting.

### M3 — Classical models and when to use them over LLMs

**Q3.1 "Random forest versus gradient boosting: how do they differ and which would you try first on a tabular dataset?"**
- Ideal answer outline:
  - Random forest: many deep trees trained independently on bootstrap samples with feature subsampling; averages to reduce variance; robust with little tuning; OOB estimate.
  - Gradient boosting: shallow trees added sequentially, each fitting the residual (gradient) of the loss; reduces bias; more sensitive to hyperparameters (learning rate, depth, number of trees, subsampling) and to noise.
  - Practical: boosted trees (XGBoost, LightGBM, CatBoost) typically win on tabular data; random forest a strong low-effort baseline.
  - Neither needs feature scaling; handle non-linearity and interactions; categorical handling differs by library (CatBoost and LightGBM have native categorical support; XGBoost also supports categoricals natively via `enable_categorical` with the `hist`/`approx` tree methods, since 1.5; other estimators need encoding unless they document native support; verified 2026-10).
  - Interpretability via feature importance, permutation importance and SHAP, with caveats about correlated features.
  - Always compare with a regularised linear baseline and use early stopping for boosting.
- Common weak answers: "boosting is just a bigger forest"; "boosting cannot overfit"; claims deep learning beats both on tabular data by default.
- Follow-ups: Which hyperparameters would you tune first for boosting? Why can impurity-based feature importance mislead? How do you handle high-cardinality categoricals?
- Difficulty: medium. Tag: `ml.classical-models`. Cluster: global-product / Indian-product.

**Q3.2 "We need to classify 2 million support tickets per day into 40 categories. Would you use an LLM?"**
- Ideal answer outline:
  - Ask: labelled data availability, accuracy bar, latency, cost budget, explainability, categories stable or changing, privacy.
  - Cost arithmetic: tokens per ticket times daily volume times price; compare to a classical or embeddings-plus-classifier model's near-zero marginal cost and millisecond latency (state assumptions, not prices).
  - Recommended hybrid: use an LLM to bootstrap labels and handle the long tail or low-confidence cases; distil into a fine-tuned small model or TF-IDF/embedding classifier for the bulk.
  - Evaluate on a stratified labelled sample including rare classes; measure per-class precision and recall.
  - Confidence routing: classical model first; defer to LLM or human when below a threshold.
  - Monitoring for drift as ticket topics change; versioning of labels and models.
  - Mention when a pure LLM wins: few labels, frequently changing taxonomy, low volume, need for rationales.
- Common weak answers: "always LLM, it is smartest"; "never LLM"; no cost estimate; no evaluation plan.
- Follow-ups: How would you pick the confidence threshold? What if a new category appears next week? How do you keep the LLM labelling consistent?
- Difficulty: medium to hard. Tag: `ml.model-choice`, `ml.classical-models`. Cluster: AI-startup / Big4 / global-IT-consulting.

**Q3.3 "Explain k-means, its assumptions and how you would choose k and validate the clusters."**
- Ideal answer outline:
  - Algorithm: initialise centroids, assign points to nearest centroid, recompute means, repeat until stable; minimises within-cluster sum of squares; k-means++ initialisation; result depends on initialisation.
  - Assumptions and limits: roughly spherical, similar-size clusters, Euclidean distance, sensitive to scale and outliers, needs k up front.
  - Choosing k: elbow, silhouette score, stability across seeds, and, most importantly, usefulness to the business decision.
  - Validation without labels: internal metrics, stability, profile clusters on features not used for clustering, downstream task impact.
  - Alternatives: Gaussian mixtures (soft clusters), DBSCAN (arbitrary shapes, noise), hierarchical.
  - Scale features; consider PCA first for high dimensions.
- Common weak answers: no mention of scaling; treats elbow as definitive; "clusters are the truth".
- Follow-ups: What happens with categorical features? How does k-means behave in very high-dimensional embedding space? How would you scale to 50 million points?
- Difficulty: medium. Tag: `ml.classical-models`. Cluster: IT-services / Big4.

### M4 — Evaluation metrics

**Q4.1 "A fraud model reports 99.5% accuracy. Is that good? What metrics would you look at?"**
- Ideal answer outline:
  - Ask the base rate: if fraud is 0.5%, predicting 'not fraud' always yields 99.5%.
  - Use the confusion matrix, precision, recall, F1 or F-beta (weight recall if misses are costly), PR curve and average precision; ROC-AUC with caveat on imbalance.
  - Tie threshold to cost: cost of missed fraud versus cost of review and customer friction; choose operating point by expected cost or by a review-capacity constraint (precision at top-k).
  - Evaluate by segment and over time; check calibration if scores drive decisions.
  - Estimate uncertainty (bootstrap) since positives are few.
  - Plan online monitoring with delayed labels.
- Common weak answers: "yes, very good"; only F1 without cost; no mention of threshold.
- Follow-ups: Why can ROC-AUC look high while precision is poor? How do you evaluate if labels are delayed 60 days? What is the effect of resampling on calibration?
- Difficulty: easy to medium. Tag: `ml.metrics`. Cluster: Indian-product (payments) / global-product / Big4.

**Q4.2 "Explain ROC-AUC in plain terms and say when you would prefer PR-AUC."**
- Ideal answer outline:
  - ROC plots true positive rate versus false positive rate across thresholds; AUC equals the probability that a random positive is scored above a random negative; 0.5 is chance.
  - Threshold-independent and class-prior independent in terms of ranking quality.
  - With heavy imbalance, false positive rate stays small even with many false positives, so ROC can look flattering; PR focuses on precision which exposes that.
  - PR-AUC baseline equals the positive rate, so it is not comparable across datasets with different rates.
  - Neither tells you the right threshold or calibration.
  - Mention partial AUC or restricting to a useful operating region.
- Common weak answers: "area under the curve" with no interpretation; claims AUC is accuracy-like; says ROC is always superior.
- Follow-ups: Can a model have a better AUC but worse performance at your operating point? How would you compare two models when curves cross? How does AUC relate to Gini?
- Difficulty: medium. Tag: `ml.metrics`. Cluster: global-product / AI-startup.

**Q4.3 "How would you evaluate a search or recommendation ranking model offline? What are NDCG and MRR and what are their blind spots?"**
- Ideal answer outline:
  - Labels: graded relevance judgements or click-derived proxies (with position bias).
  - Precision@k and recall@k for set-based views; MRR for tasks with one right answer (rank of first relevant); MAP for binary relevance across list; NDCG for graded relevance with position discount and normalisation against the ideal ordering.
  - Choose k based on what the UI shows.
  - Blind spots: position bias in click data, unjudged items treated as irrelevant, diversity and freshness not measured, offline-online mismatch.
  - Complement with online A/B tests, interleaving, and guardrail metrics.
  - For retrieval in RAG, link to recall@k as the first-stage ceiling (pointer to B3).
- Common weak answers: uses accuracy; cannot define NDCG; ignores position bias.
- Follow-ups: Compute NDCG@3 for a given list. How would you collect judgements cheaply? Why might offline NDCG improve while online engagement falls?
- Difficulty: medium to hard. Tag: `ml.metrics`. Cluster: global-product / Indian-product / AI-startup.

### M5 — Data quality, leakage and feature engineering

**Q5.1 "What is data leakage? Give three concrete examples and explain how you would catch each."**
- Ideal answer outline:
  - Definition: information unavailable at prediction time influences training or validation, inflating offline performance.
  - Example 1, target leakage: a feature derived after the outcome (refund issued predicting churn); catch with feature timing review and suspicious importance.
  - Example 2, preprocessing leakage: scaling, imputation or target encoding fit on all data before split; fix by fitting inside a pipeline within each fold.
  - Example 3, temporal or entity leakage: random split of time-ordered or per-user data; fix with time-based or group splits.
  - Also duplicates across train and test; oversampling before splitting.
  - Detection signals: too-good scores, one dominant feature, drop in performance with temporal split; reconstruct the data as of prediction time ("point-in-time correctness").
  - Prevention: feature definitions with timestamps, pipelines, review checklists.
- Common weak answers: only "test data in training"; cannot give examples; no prevention process.
- Follow-ups: How does point-in-time correctness work with a feature store? Why is oversampling before CV a leak? Where does target encoding leak and how do you fix it?
- Difficulty: medium. Tag: `ml.data-quality`. Cluster: global-product / Big4 / AI-startup.

**Q5.2 "Training used SQL to compute features; serving uses Python. The model degrades silently. What's going on and how do you prevent it?"**
- Ideal answer outline:
  - Train-serve skew: two implementations drift (null handling, time windows, rounding, categorical vocab, timezone).
  - Detect: log served features and compare distributions with training; replay and diff on a sample; unit tests with golden inputs.
  - Prevent: single feature definition used for both (shared library, feature store, or compute features in one place and store them), serialised preprocessing in a `Pipeline`, schema validation, versioned feature sets.
  - Monitor input drift (PSI, KS), missing-rate spikes, and model output distribution.
  - Backend-engineer framing: treat features as an API contract with tests and versioning.
  - Rollback plan and shadow deployment before cutover.
- Common weak answers: "retrain more often"; blames the algorithm; no concrete detection.
- Follow-ups: What would you log, and with what privacy considerations? How would you test point-in-time joins? Online versus batch features: what changes?
- Difficulty: medium to hard. Tag: `ml.data-quality`. Cluster: global-product / Indian-product.

**Q5.3 "How do you handle a dataset with 1% positives?"**
- Ideal answer outline:
  - First, pick metrics suited to imbalance (PR, recall at precision, cost-based) and stratified splits.
  - Options: class weights or cost-sensitive loss; resampling the training fold only (undersample majority, oversample, SMOTE with caveats); threshold moving; ensembles built for imbalance; anomaly-detection framing when positives are extremely rare.
  - Never resample validation or test sets; do it inside CV folds.
  - Resampling distorts probability calibration: recalibrate if probabilities are used.
  - Collect more positives or better labels if feasible; investigate label quality.
  - Evaluate per segment, and with confidence intervals.
- Common weak answers: "just SMOTE" and report accuracy; oversample before the split; no calibration mention.
- Follow-ups: Why does SMOTE struggle with categorical or high-dimensional data? When prefer class weights over resampling? How would you recalibrate?
- Difficulty: medium. Tag: `ml.data-quality`, `ml.metrics`. Cluster: IT-services / Indian-product.

### M6 — NumPy and Pandas fluency

**Q6.1 "Explain NumPy broadcasting and give an example where a mistake in shapes silently gives the wrong answer."**
- Ideal answer outline:
  - Rule: align shapes from the trailing dimension; dimensions are compatible if equal or one is 1; size-1 dimensions stretch without copying data.
  - Example: subtracting a per-row mean needs `keepdims=True` or a reshaped column; subtracting a length-n vector from an (n, n) matrix subtracts along the wrong axis if you meant rows.
  - A (n,) minus (n, 1) produces an (n, n) matrix, not elementwise, a classic silent bug in distance computations.
  - Verify with `.shape` assertions and small test arrays.
  - Memory: broadcasting is a view-like operation but the result is materialised; huge intermediate arrays can exhaust memory (pairwise distances), so chunk it.
  - Benefit: vectorisation replaces Python loops with C-level loops.
- Common weak answers: "it repeats the array" without rules; never mentions shape checks; cannot explain `axis`.
- Follow-ups: How would you compute pairwise cosine similarity between 100k queries and 1M documents in memory-safe manner? What does `keepdims` do? View versus copy: when does slicing copy?
- Difficulty: medium. Tag: `data.numpy-pandas`. Cluster: global-product / AI-startup / Indian-product.

**Q6.2 "Given an events table (user_id, timestamp, event), how would you compute, in Pandas, each user's most recent event and a 7-day rolling count of events per user? Discuss pitfalls."**
- Ideal answer outline:
  - Ensure timestamp dtype is datetime and sorted; sort by user and time.
  - Most recent event: `sort_values` then `groupby(...).tail(1)` or `idxmax` on the timestamp per group, mention tie behaviour.
  - Rolling count: set the time index and use `groupby('user_id').rolling('7D')` on a datetime index with the right closed side, or a `merge_asof` or window approach; mention that rolling by time requires a monotonic index.
  - Beware of leakage: the rolling window must exclude future rows and often the current row for prediction-time features (shift).
  - Performance: avoid `apply` with Python lambdas; consider categorical dtype for user ids; for very large data move to SQL window functions, DuckDB or Polars (awareness).
  - Validation: assert no duplicated users in the result; test with a tiny hand-checked frame.
- Common weak answers: iterates with `iterrows`; forgets sorting; ignores timezone and leakage.
- Follow-ups: What changes if timestamps have timezones? How do you handle users with one event? How would this look in SQL?
- Difficulty: medium. Tag: `data.numpy-pandas`. Cluster: IT-services / Indian-product / global-product.

**Q6.3 "A merge in Pandas doubled your row count. What happened and how do you prevent it?"**
- Ideal answer outline:
  - Cause: duplicate keys on one or both sides produce many-to-many joins (cartesian within key groups); also unintended key columns or NaN keys matching.
  - Diagnosis: check key uniqueness (`duplicated`, `value_counts`), compare row counts before and after, inspect rows from one key.
  - Prevention: `validate='one_to_one'`/`'many_to_one'` in `merge`, `indicator=True` to audit match types, deduplicate with explicit rules before joining.
  - Choose the join type deliberately (inner, left, anti-join via indicator).
  - Document and test cardinality assumptions; in a pipeline assert row counts.
  - Relate to SQL join semantics, which the learner already knows.
- Common weak answers: "just drop duplicates afterwards" (hides data errors); no validation parameter.
- Follow-ups: How do NaN keys behave in a merge? How would you find which keys exploded? What differences exist between `join` and `merge`?
- Difficulty: easy to medium. Tag: `data.numpy-pandas`, `ml.data-quality`. Cluster: IT-services / Big4.

### M7 — Neural network basics and training mechanics

**Q7.1 "Explain how a neural network is trained, from forward pass to weight update."**
- Ideal answer outline:
  - Forward pass: inputs flow through layers (linear transforms and non-linear activations) to produce predictions.
  - Loss measures error (cross-entropy for classification, MSE for regression).
  - Backpropagation applies the chain rule to compute the gradient of the loss with respect to each parameter efficiently, reusing intermediate values.
  - Optimiser uses gradients to update weights (SGD, momentum, Adam/AdamW) scaled by a learning rate.
  - Mini-batches approximate the full gradient; epoch means a full pass over data.
  - Practical controls: learning-rate schedule and warm-up, weight decay, dropout, normalisation, early stopping on validation loss.
  - Debug habit: overfit a tiny batch first; monitor loss and gradient norms.
- Common weak answers: "backprop updates the weights"; cannot link loss to the gradient; mixes epoch and batch.
- Follow-ups: Why do we need non-linear activations? What happens if the learning rate is too high? Why is Adam popular, and what is AdamW's difference?
- Difficulty: medium. Tag: `ml.deep-learning-basics`. Cluster: global-product / AI-startup.

**Q7.2 "What is the difference between training and inference for a large model in terms of compute and memory?"**
- Ideal answer outline:
  - Training: forward plus backward pass, needs gradients, optimiser state, stored activations; memory is several times the weight size; compute per token is a multiple of the forward cost (state it qualitatively; exact multipliers depend on setup and are not memorised); requires distributed hardware for large models.
  - Inference: forward only, no gradients, weights plus KV cache for generation; can be quantised; throughput via batching.
  - Training is a one-off or periodic cost; inference is a per-request recurring cost that dominates at scale.
  - For LLMs, decode is memory-bandwidth bound; prefill is compute bound; latency metrics TTFT and tokens per second.
  - Optimisation levers: quantisation, batching, caching, smaller or distilled models, speculative decoding.
  - Tie to cost decisions: when fine-tuning a small model beats calling a large API (pointer to B7).
- Common weak answers: "inference is just training without a loss"; ignores memory components; no latency notions.
- Follow-ups: Which memory components exist in training but not in serving, and which dominate for a long-context workload (worked arithmetic is B7.M8)? Why does batching help throughput but hurt latency? What is the KV cache?
- Difficulty: medium to hard. Tag: `ml.deep-learning-basics`, `ml.transformers`. Cluster: global-product / AI-startup.

**Q7.3 "Your network's training loss isn't going down. How do you debug it?"**
- Ideal answer outline:
  - Verify the pipeline: labels aligned with inputs, normalised inputs, shuffling, no NaNs.
  - Sanity checks: overfit a single small batch; check initial loss against expected value (for example log of number of classes).
  - Learning rate: too high (loss oscillates or NaN) or too low (flat); try a range test or sweep.
  - Check gradient flow: vanishing or exploding gradients, activation saturation, initialisation, add normalisation or residuals, gradient clipping.
  - Simplify the model, then scale up; check the loss function and output activation pairing.
  - Compare with a classical baseline to confirm there is signal.
  - Use a fixed seed and logging for reproducibility.
- Common weak answers: "add more layers" or "train longer" without checks.
- Follow-ups: What does an initial loss far above log(C) suggest? How would you detect vanishing gradients? Loss drops but validation loss rises: now what?
- Difficulty: medium. Tag: `ml.deep-learning-basics`. Cluster: AI-startup / global-product.

### M8 — Embeddings and tokenisation

**Q8.1 "What is an embedding and how do you decide between cosine similarity, dot product and Euclidean distance?"**
- Ideal answer outline:
  - An embedding is a dense vector representation learned so that semantically or functionally similar items are close.
  - Cosine measures angle and ignores magnitude; for unit-normalised vectors cosine equals dot product, and Euclidean distance is a monotonic function of cosine, so rankings agree.
  - Use the metric the embedding model was trained with; many modern text embedding models are trained with cosine or dot product (check documentation).
  - Dot product retains magnitude information, which can encode popularity or confidence in recommenders.
  - Practical: normalise once at index time; keep the same model for queries and documents (or the paired query/document variant).
  - Limits: domain shift, negation and numbers, versioning requires re-embedding.
  - Evaluate on your own labelled queries with recall@k, not on generic benchmarks alone.
- Common weak answers: "cosine is always best"; says embeddings capture meaning exactly; does not know normalisation effects.
- Follow-ups: What breaks when you upgrade the embedding model? How does dimensionality affect storage and speed? How would you evaluate an embedding model for your domain?
- Difficulty: medium. Tag: `ml.embeddings`. Cluster: AI-startup / global-product.

**Q8.2 "Why do LLMs use subword tokenisation, and what are the practical consequences for an application developer?"**
- Ideal answer outline:
  - Word-level vocabularies are huge and cannot handle unseen words; character-level sequences are too long; subwords (BPE, WordPiece, SentencePiece) balance vocabulary size and sequence length and handle rare words via composition; byte-level variants avoid unknown tokens.
  - BPE idea: iteratively merge the most frequent adjacent pairs.
  - Consequences: cost and latency are counted in tokens; context limits are in tokens; the same text has different token counts across models and languages (non-English and code can cost more); counting must use the model's own tokeniser.
  - Behaviour quirks: spelling, character counting, arithmetic and rare strings are harder; whitespace and formatting change token counts.
  - Operational: truncate by tokens not characters; budget output tokens; chat templates add special tokens.
  - Pointer to B2 for context and cost optimisation.
- Common weak answers: "a token is a word"; "about four characters for everything"; ignores language differences.
- Follow-ups: How would you estimate the cost of processing 10 million Hindi documents? Why can an LLM miscount letters in a word? What is a special token?
- Difficulty: medium. Tag: `ml.embeddings`, `ml.transformers`. Cluster: AI-startup / Indian-product / global-IT-consulting.

**Q8.3 "You have 50 million document embeddings and need low-latency similarity search. At a conceptual level, what is the trade-off space and how would you decide?" (concept depth only; index internals are B3.M4)**
- Ideal answer outline:
  - Exact search costs O(N·d) per query: fine for small corpora or brute force at moderate scale; this is the correctness reference.
  - Approximate search trades recall for latency and memory: the interviewer wants the triangle (recall, latency, memory) plus build and update cost, not index internals.
  - Say that you would measure recall of the approximate result against exact search on a sample of real queries, and track latency percentiles.
  - Cheaper levers at concept level: smaller or compressed vectors, normalising once, metadata filtering interacts with recall.
  - Name that specific index families and parameter tuning exist and that you would pick by measurement; defer details (HNSW, IVF, pgvector) to the retrieval course.
  - Link to the backend lens: it is a cache-versus-accuracy decision with a quantified error budget, like a stale-read tolerance.
- Common weak answers: "use a vector DB" with no reasoning; ignores recall measurement; no mention of memory; recites index names without trade-offs.
- Follow-ups: How would you measure how much recall you lose? What changes when a metadata filter is applied? What happens to the index when you re-embed with a new model?
- Difficulty: medium. Tag: `ml.embeddings`. Cluster: global-product / AI-startup. `ref`: deeper treatment `Q-B3-M4-*`.

### M9 — Transformers, attention, training versus inference

**Q9.1 "Explain self-attention step by step. Why queries, keys and values, and why scale by the square root of the key dimension?"**
- Ideal answer outline:
  - Each token embedding is projected into a query, a key and a value by learned matrices.
  - Attention scores are query-key dot products, measuring relevance of every token to every other; scaled, softmaxed into weights; output is the weighted sum of values.
  - Intuition: query asks, key advertises, value carries content; this lets each token build a context-dependent representation.
  - Scaling by root d_k keeps dot-product variance stable so softmax does not saturate (tiny gradients) at larger dimensions.
  - Multi-head: several attention operations in parallel with different projections, concatenated and projected; lets the model attend to different relations.
  - Causal mask in decoders prevents looking at future tokens; positional information must be added because attention itself is order-agnostic.
  - Cost: n by n score matrix, O(n^2) time and memory in sequence length.
- Common weak answers: cannot name Q, K, V roles; says attention is "focus" without mechanics; forgets masking and positions.
- Follow-ups: What would happen without positional encoding? How does multi-query or grouped-query attention reduce cost? Why do decoder-only models use causal masks during training too?
- Difficulty: medium to hard. Tag: `ml.transformers`. Cluster: global-product / AI-startup.

**Q9.2 "What is the KV cache, and how does it affect cost, latency and memory when serving long contexts?"**
- Ideal answer outline:
  - During autoregressive generation, keys and values of previous tokens are reused at each step; caching them avoids recomputing the prefix.
  - Memory grows linearly with sequence length, number of layers, heads (KV heads) and batch size, times head dimension and precision; for long contexts and large batches it can rival or exceed the weights.
  - Prefill processes the prompt in parallel (compute-bound); decode generates one token at a time (memory-bandwidth-bound, reads the whole cache each step).
  - Levers: grouped-query or multi-query attention (fewer KV heads), quantised KV cache, paged attention and continuous batching for efficient memory use, prompt/prefix caching to reuse shared prefixes.
  - Application impact: long prompts raise time-to-first-token; repeated shared prefixes benefit from provider prompt caching (pointer to B2); output length drives total latency.
  - Be able to do a back-of-envelope calculation given layer count, heads and dimensions.
- Common weak answers: "it caches responses"; conflates with application-level caching; no memory scaling.
- Follow-ups: Compute the KV cache for given dimensions and a 32k context. Why does decode speed depend on memory bandwidth? What is prefix caching?
- Difficulty: hard. Tag: `ml.transformers`, `ml.deep-learning-basics`. Cluster: global-product / AI-startup.

**Q9.3 "Encoder-only, decoder-only and encoder-decoder transformers: what are they, and which would you choose for classification, embeddings, summarisation and chat?"**
- Ideal answer outline:
  - Encoder-only (BERT-style): bidirectional attention, strong for understanding tasks, classification, token tagging and as the basis of many embedding models.
  - Decoder-only (GPT-style): causal attention, next-token generation; dominant for general-purpose LLMs and chat; also used for in-context learning.
  - Encoder-decoder (T5/translation style): encoder reads input, decoder generates conditioned output; good for translation, summarisation with fixed input-output structure.
  - Choice: classification at scale with labels, small fine-tuned encoder is cheap and fast; embeddings, encoder-based embedding model; chat and open-ended generation, decoder-only LLM; summarisation either an LLM or encoder-decoder depending on quality, cost and control.
  - Mention training objectives: masked language modelling versus next-token prediction.
  - Be honest that boundaries blur: decoder-only LLMs are also used for embeddings and classification.
- Common weak answers: lists names without mechanics; "BERT is old so never use it".
- Follow-ups: Why is a small encoder often cheaper than an LLM for classification? What is masked language modelling? Why did decoder-only models win for general-purpose assistants?
- Difficulty: medium. Tag: `ml.transformers`, `ml.model-choice`. Cluster: global-IT-consulting / AI-startup / Big4.

### M10 — Experiment design, A/B testing and the ML-versus-LLM decision

**Q10.1 "Design an A/B test for a new recommendation model. What can go wrong?"**
- Ideal answer outline:
  - Hypothesis and primary metric (for example, conversion or watch time per user), plus guardrails (latency, complaints, revenue, diversity).
  - Randomisation unit (user, not request) with stable assignment; exclude contamination.
  - Sample size from baseline rate, minimum detectable effect, alpha and power; planned duration covering weekly cycles.
  - Pre-checks: A/A test, SRM check on allocation.
  - Analysis: pre-registered metric, confidence intervals, avoid peeking or use sequential methods; adjust for multiple comparisons.
  - Pitfalls: novelty effect, network or marketplace interference, seasonality, metric selection bias, instrumentation bugs.
  - Rollout: ramp 1%, 5%, 25%, 50%, with kill switch; long-term holdout.
  - Offline evaluation and shadow mode precede the online test.
- Common weak answers: no sample size; randomises by request; stops when p less than 0.05; no guardrails.
- Follow-ups: What do you do when the primary metric is flat but guardrail worsens? How do you handle a low-traffic product? What is SRM and what do you do if it fails?
- Difficulty: medium to hard. Tag: `ml.experiment-design`. Cluster: global-product / Indian-product.

**Q10.2 "Your A/B test shows p = 0.04 after day 3 of a planned 14-day run. Do you ship?"**
- Ideal answer outline:
  - No, not on that evidence: repeated looks inflate false-positive rate (peeking); the planned sample size is the stopping rule unless a sequential design was set up.
  - Early effects can be novelty-driven and weekday-biased; a full weekly cycle is needed.
  - A p-value is not the probability the hypothesis is true; report effect size with confidence interval and practical significance.
  - Check SRM, guardrails and segment consistency; avoid fishing across segments.
  - If early stopping is a business need, use group-sequential or always-valid methods designed up front.
  - Document the decision and follow up post-launch.
- Common weak answers: "p less than 0.05, ship"; "wait for p to hit 0.01"; confuses significance with importance.
- Follow-ups: How much does peeking inflate the error rate, and how would you demonstrate it? What is the minimum detectable effect and why does it matter? What is the difference between statistical and practical significance?
- Difficulty: medium. Tag: `ml.experiment-design`. Cluster: Indian-product / global-product / Big4.

**Q10.3 "A client wants a document-triage system for 5 million scanned invoices a year with 20 fields to extract. Walk through how you choose between classical ML, fine-tuned models and an LLM, and how you evaluate."**
- Ideal answer outline:
  - Clarify: layout variety, language mix, accuracy and audit needs, latency, privacy and data residency, existing labelled data, human-review capacity.
  - Options: OCR plus rules or templates for fixed layouts; layout-aware or fine-tuned extraction models for high volume and stable fields; multimodal or text LLM for varied layouts and low volume; hybrid with confidence routing to human review.
  - Cost model: per-document cost and latency at 5 million per year, with assumptions explicit; volume favours cheaper models for the bulk.
  - Evaluation: field-level precision/recall/exact-match on a stratified labelled sample including hard layouts; business cost of each field error; confidence calibration to choose automation rate.
  - Governance: PII handling, audit trail, versioning of prompts and models, drift monitoring when vendors change formats.
  - Rollout: pilot, shadow mode against humans, then partial automation with sampling QA.
  - State the recommendation and what evidence would change it.
- Common weak answers: "use an LLM for everything" or "build our own model"; no cost numbers; no human-in-the-loop; no field-level metric.
- Follow-ups: How do you pick the automation threshold? What happens when a vendor changes the invoice format? How would you create labels cheaply to start?
- Difficulty: hard. Tag: `ml.model-choice`, `ml.experiment-design`. Cluster: Big4 / global-IT-consulting / IT-services.

Total seeded verbal questions: 30 (3 per module).

## 5. Final assessment

| Component | Description |
|---|---|
| Mastery check | 24 items sampled across all ten modules, weighted towards M2, M4, M5 and M9 (the most-probed areas); timed 40 minutes; pass at 80%; misses route to a revision loop with targeted micro-lessons. |
| Timed data task | 45 minutes in the browser IDE with hidden tests: given a messy events and customers dataset, clean it, build leak-free features with a `Pipeline`, train and compare a baseline and a boosted model with an appropriate time-based split, report PR-AUC, precision at a chosen threshold and a cost-based threshold justification, and include a short written note on leakage risks. Evaluated on correctness, absence of leakage, vectorised idioms and the quality of the written reasoning. |
| Voice mock round | 20 minutes with the Interview Master, adaptive follow-ups, covering framing, metrics, leakage, transformers and an ML-versus-LLM decision. Scored on the rubric (technical accuracy, structure, trade-off reasoning, honesty about uncertainty, concise delivery) with evidence quotes. Badge needs "Meets" (average >= 3.0, none < 2); readiness needs the Senior bar (average >= 3.5, none < 3) in two sessions at least 48 hours apart (G-6, G-7). |
| Design or canvas task (optional for the badge; counts toward readiness; kept outside the 2.0 h allowance, which covers the 40 + 45 + 20 minute sittings plus revision loops) | 25 minutes: draw and defend the architecture of an ML-powered ticket-routing feature (data pipeline, features, model tier with LLM fallback, confidence routing, monitoring, retraining trigger, experiment plan). The rule engine cites registry IDs only (for example R-OBS-1 observability coverage, R-AI-1 fallback on the LLM tier, R-COST-1 cost driver noted; stated non-goals are suppressed); train-serve parity, drift monitoring, rollback path, evaluation plan and cost reasoning are rubric-scored dimensions, not locally invented rules (boss matrix section 7). |
| Certificate rule | Course badge awarded when all module badges are held, the final mastery check (80%) and timed task pass (at least 70% of hidden tests plus a written leakage note at rubric >= 3.5), and the voice mock reaches "Meets"; otherwise a targeted study plan is generated. Interview-ready status is reported separately per tag (LF-2). |
| Design and canvas rules | The ticket-routing canvas task uses registry rule IDs only (`R-AI-*` and the relevant reliability and cost rules owned by A6); B1 does not define rules. Rubric overall >= 3.5, no dimension < 3 (G-7). |

Style-of-question examples across the course (not attributed to any company): why is accuracy misleading; how do you detect leakage; explain bias-variance; walk through attention; why might an A/B test be invalid; classical model or LLM for this task.

## 6. Course-level risks and freshness notes

| Risk or item | Note |
|---|---|
| Fast-moving model landscape | Transformer variants (grouped-query attention, mixture-of-experts, long-context methods, state-space hybrids) and model families change quickly. Keep M9 conceptual and architecture-agnostic; date all named model examples; avoid quoting context lengths, parameter counts or prices as facts. Review every 3 months. |
| Library versions (checked October 2026) | Confirmed against official sources (PyPI metadata, pandas whatsnew; verified 2026-10): NumPy 2.4 supports Python 3.11+ and NumPy 2.5.x (June 2026) requires 3.12+, both with cp314 wheels; pandas 3.0 (2026-01-21) made copy-on-write the default and only mode and added a default dedicated string dtype; scikit-learn 1.8 (December 2025) and 1.9.x (current 1.9.1) support Python 3.11-3.14; PyTorch 2.10 to 2.14 ship cp314 wheels (2.14.x current); B1 does not use or teach free-threaded builds. Pins live in the platform manifest and every example is CI-tested. XGBoost 3.4.x, LightGBM 4.7.0 and CatBoost 1.2.10 releases on Python 3.14 verified 2026-10 via PyPI metadata (the course uses scikit-learn's histogram gradient boosting as the guaranteed baseline). |
| Pandas ecosystem shifts | Pandas 3.0 behaviour changes (copy-on-write, string dtype) are taught as current, with a legacy-behaviour note for old codebases. Polars and DuckDB stay awareness only; positioning claims kept qualitative. |
| Experiment-tracking and data-validation tools | Tool landscape changes (MLflow, Weights and Biases, Great Expectations, Pandera, Evidently and others): teach the concepts, mention tools as examples only, no feature claims. Tool names and status verified 2026-10 (MLflow 3.x, Weights and Biases under CoreWeave since May 2025, Great Expectations 1.x, Pandera 0.33, Evidently 0.7.x); re-check at each quarterly review. |
| Tokeniser behaviour | Token counts and multilingual cost ratios vary by model generation; every numeric comparison is generated live in the IDE, never hard-coded. The `tiktoken` library is MIT-licensed code (verified 2026-10), but its vocabulary files are downloaded at runtime (from openaipublic.blob.core.windows.net) and must be primed into an offline cache directory (`TIKTOKEN_CACHE_DIR`) for the sandbox (verified 2026-10). `[verify: redistribution licence of the vocabulary files not stated in any primary source found; engineering and legal, spike with P-01]`. |
| "Lost in the middle" and long-context claims | Taught as a hedged, model-dependent finding; learners are told to measure their own model. No numeric claims. Re-checked each quarterly review. |
| Training memory multipliers and compute rules of thumb | Removed as numeric facts (boss ruling). Memory components are taught qualitatively; GPU arithmetic and calculators are B7.M8. The only formula taught as exact is the KV-cache size formula, which is arithmetic, not a rule of thumb. |
| Scope creep | Risk of turning into a full ML degree. Guardrail: every topic must map to an interview tag and a seeded or banked question; maths limited to what is explained verbally. |
| Overlap with other courses | Retrieval metrics and vector search (B3), eval design for LLM features (B5), fine-tuning and serving optimisation (B7), tokens and cost in application code (B2). B1 owns the underlying concepts; others link back rather than re-teach. |
| Compute and sandbox limits | Embedding and PyTorch demos must run on CPU within sandbox time and memory limits; use small models or precomputed vectors. |
| Interview realism | Questions are style-of-question only; no claims about what specific companies ask; Indian-context examples (UPI-style payments fraud, regional-language token costs) are illustrative. |
| Review cadence | Stable theory: 12 months. M6 library behaviour, M8 tokenisation, M9 and ML-versus-LLM sections: 3 months. Version stamp, "as of" date and last-reviewed date (October 2026) on every lesson. |

## 7. Open items for the boss agent

The five wave-1 questions are closed by R-B1-1 to R-B1-5. Remaining real items:

1. Tokeniser vocabulary files: confirm legal redistribution terms for bundling offline (engineering spike with P-01); fallback is a toy-BPE-only lab plus learner-pasted counts.
2. Interpretation of the AI-route weight for B1 (`M`) versus the reference learner's L1-L2 status: confirm that the adaptive engine may place B1 early (week 1-3 thread) given the dependency of B2, B3, B5 and B7 on it. Partly addressed: the Learner Journey doc already places B1 in phase P1 (weeks 1-2); the open point is only the reference-learner hours (LF-1 says about 15 h plus assessment, the Journey doc says 8.5 h).
3. Boss compliance note: instruction 1 tags M8-M9 `H:M` and M1-M5, M7, M10 `H:S`; this file also gives M9 and M10 hybrid interview weight above 15 percent, which `[judgement]` may justify a `H:M` for M10 after the pilot.

## 8. Learner-Fit Appendix (G-11, template section 6)

Tag format `B:x A:x H:x` where B = Senior Python Backend, A = AI Engineer, H = hybrid; `M` MUST, `S` SHOULD, `N` NICE. Reference learner: senior backend engineer (11+ years, about 4 in Python), L1-L2 in ML and statistics, strong in FastAPI, Django, Flask, SQL, Redis, Docker, AWS Lambda/S3, MQTT, LLM APIs and voice agents.

### LF-1 Level paths

| Level | Route and hours | Practice mix | Skipped | Challenge items |
|---|---|---|---|---|
| L1 (new to ML) | Simpler, 19.5 h plus 2.0 h assessment; all lessons, maths and Pandas refreshers, prerequisites linked | 60% guided katas and numeric walk-throughs, 30% predict-the-output, 10% verbal | Nothing; M6 refreshers first | None until each mastery check passes |
| L2 (junior-mid, uses ML with guidance) | Simpler with known lessons skimmed, about 15-17 h | 45% coding, 30% predict-the-output, 25% verbal and scenario | Lessons scoring >= 85 on diagnostics | Calibration plot, ROC-versus-PR comparison |
| L3 (uses ML independently, cannot always explain it) | Deeper, about 11-13 h net | 40% verbal and scenario, 40% coding, 20% predict | Foundations (M1, M6, parts of M3) per band | Boosting regularisation, nested CV, paired bootstrap |
| L4 (owns ML in production) | Deeper plus Challenge, 8-11 h; mastery checks first | 60% verbal and scenario (senior-bar framing), 30% timed coding, 10% review | Everything scoring >= 85 | Metric-gaming critique, SRM and CUPED reasoning, KV-cache design review |

The reference learner is L4 in the backend contexts used for bridges but L1-L2 in ML: expect "study" or "deep-study" bands on M2, M3, M4, M7, M9, M10 stats and "skip" or "skim" on M6 and the Pandas parts of M5. Deeper stays meaningful for L3/L4 ML learners through the Challenge items above. Expected time for the reference learner: the 13 h Deeper figure assumes L3/L4 competence in the topic, which this learner does not have in ML. With most modules in the "study" band (taken as the midpoint of the Simpler and Deeper hours), M6 reduced to its mastery check and M1 skimmed, plan for about 15 h plus the 2.0 h assessment allowance `[judgement; calibrate after the 3-learner pilot]`. Study-band and deep-study modules are never timed at Deeper hours.

### LF-2 Module matrix

Diagnostics are 6 placement items per module (8 for the standalone M6), `Q-B1-M<n>-P1..Pk` (from the MCQ and predict buckets; 62 in total). Bands (G-5): >= 85 skip lessons and take the mastery check (plus optional Challenge); 70-84 skim Deeper-only lessons plus the check; 50-69 study; < 50 deep-study (Simpler plus remediation). Interview weight is percent of target-role ML-fundamentals interview time `[judgement]`, calibrated after the pilot. B1 serves the AI Engineer ML round (judgement: transformers, metrics and leakage dominate), the backend route only through data-aware design questions.

| Module | Diag. | Tags | Weight B / A / H (%) | Rationale (judgement) | Rehearsal modes | Module readiness criterion |
|---|---|---|---|---|---|---|
| M1 Framing | 6 | B:N A:M H:S | 10 / 8 / 6 | Opening framing question in most ML rounds, rarely deep | Voice drill, design canvas (problem one-pager) | G-6 and framing answer states target, action, baseline, metric, split |
| M2 Generalisation | 6 | B:N A:M H:S | 8 / 10 / 9 | Bias-variance and CV are standard probes | Voice drill, scenario | G-6 and diagnoses a described curve pair correctly in under 90 s |
| M3 Classical models | 6 | B:N A:M H:S | 10 / 10 / 9 | "Classical vs LLM" is a frequent senior question | Voice drill, scenario | G-6 and gives a justified model choice with cost and latency for 3 cases |
| M4 Metrics | 6 | B:N A:M H:S | 12 / 12 / 11 | Most probed fundamentals topic: imbalance, thresholds, ranking | Voice drill, coding (metrics from scratch) | G-6 and computes P/R/F1 and NDCG by hand correctly and picks a cost-based threshold |
| M5 Leakage and data quality | 6 | B:N A:M H:S | 18 / 10 / 11 | Leakage and train-serve skew are the best backend-to-ML bridge and a senior favourite | Voice drill, coding, design canvas | G-6 and finds the leak in 3 of 3 described pipelines, timed |
| M6 NumPy and Pandas | 8 (standalone) | B:N A:M H:S | 8 / 5 / 4 | Practical screening round, less in senior loops | Coding (timed 20 min) | Hidden tests >= 70% on 2 timed katas; standalone test-out available |
| M7 NN training | 6 | B:N A:M H:S | 4 / 8 / 7 | Probed at concept level for AI roles | Voice drill | G-6 and explains forward, loss, backprop, optimiser step without conflating backprop and update |
| M8 Embeddings and tokens | 6 | B:N A:M H:M | 10 / 10 / 11 | Central to RAG and LLM-app interviews | Voice drill, coding (cosine search), scenario | G-6 and cosine versus dot versus Euclidean with normalisation, plus a token-cost estimate method |
| M9 Transformers | 6 | B:N A:M H:M | 6 / 15 / 16 | Highest-weight AI Engineer fundamentals topic | Voice drill (strict), whiteboard-style explanation | G-6 and derives attention steps and KV-cache memory for given dimensions unaided |
| M10 Experiments and decision | 6 | B:N A:M H:S | 14 / 12 / 16 | A/B and ML-vs-LLM decisions are senior and consulting staples | Voice drill, design canvas, scenario | G-6 and designs an A/B test with sample size, guardrails, SRM and peeking handled |

Weights sum to 100 for each of B, A, H. Course-level readiness for the "ML fundamentals" role tag: all M-tagged modules ready (A and H routes), voice drill average >= 3.5 on `ml.*` tags in two sessions >= 48 h apart, timed data task passed.

### LF-3 Experience bridge ("you already know" -> senior-depth gap)

| Module | You already know | Gap between using and explaining at senior depth |
|---|---|---|
| M1 | Writing requirements and acceptance criteria for backend features | Choosing a prediction target, prediction point and action cost; saying when a rule beats ML |
| M2 | Load-testing and environment parity (staging versus prod), regression tests | Why a validation scheme must mimic deployment; reading learning curves; the tuning-on-test trap |
| M3 | Choosing between Postgres, Redis and queue for a workload by access pattern | Model families by data shape, scaling needs and interpretability; cost-latency-accuracy tables |
| M4 | SLOs, p95/p99, precision of alerts versus missed alerts | Thresholds from cost, PR versus ROC under imbalance, calibration, ranking metrics |
| M5 | Schema drift between services, replica lag, ETL contracts, idempotent pipelines | Leakage taxonomy, point-in-time correctness, feature parity (feature store as a cache with a freshness contract), drift as monitoring of an input contract |
| M6 | SQL joins and window functions; Python data structures | Vectorisation, broadcasting, pandas 3.0 copy semantics, join cardinality guards |
| M7 | Profiling, memory budgeting of services | What backprop is and is not, the training memory components, why training and inference costs differ |
| M8 | Hash and B-tree indexes as lookup keys; LLM API token billing | Embeddings as learned lossy keys, normalisation, tokeniser mechanics behind the bill, re-embedding migration cost |
| M9 | Using chat and tool-calling LLM APIs, latency tuning of voice agents (first-token latency) | Attention mechanics, why context is quadratic in compute, prefill versus decode, KV-cache memory, why temperature 0 is not bit-exact |
| M10 | Canary releases, feature flags, rollback, traffic splitting on AWS | Randomisation unit, power, peeking, SRM, bootstrap CIs, and the ML-versus-LLM decision with cost reasoning |

### LF-4 Senior-bar framing

| Module | Mid answer | Senior adds | Staff adds |
|---|---|---|---|
| M1 | Names a model | Defines target, action, cost asymmetry, baseline, split, monitoring | Frames portfolio-level ML versus rules decisions, feedback loops, ownership and rollout risk |
| M2 | Defines bias and variance | Diagnoses from curves, picks validation matching deployment, knows the tuning trap | Sets team validation standards and holdout policy, reviews others' CV |
| M3 | Lists algorithms | Picks with data shape, latency, interpretability and cost; knows when an LLM loses | Designs hybrid tiers with confidence routing and a distillation plan |
| M4 | Accuracy, F1 | Threshold from cost, PR under imbalance, calibration, CI by bootstrap | Aligns offline metric with product metric, guards against metric gaming organisation-wide |
| M5 | Knows "don't leak" | Taxonomy of leaks, point-in-time features, train-serve parity, drift checks | Builds contracts, ownership and CI checks so leakage cannot ship |
| M6 | Correct but loopy code | Vectorised, memory-aware, cardinality-guarded | Knows when to leave pandas and what to standardise across a team |
| M7 | Describes layers | Explains the learning loop, debugs training, knows memory components | Reasons about training-cost budgets and when not to train |
| M8 | Says cosine similarity | Normalisation equivalences, model versioning and re-embedding, token cost mechanics | Plans embedding migrations and multilingual cost at scale |
| M9 | Names Q, K, V | Derives attention cost and KV memory, prefill versus decode, serving levers | Chooses architecture and serving strategy against latency, memory and cost targets |
| M10 | States A/B basics | Power, SRM, peeking, guardrails, ML-versus-LLM cost reasoning | Runs an experimentation programme: interference, long-term holdouts, decision records |

### LF-5 Adaptive hooks

| Module | Remediation (on mastery < 80% or band < 50) | Challenge (strong learner) | Reorder or defer by engine |
|---|---|---|---|
| M1 | Probability vocabulary refresher (Simpler) | Delayed-label and feedback-loop case | May defer after M5 for L3+ |
| M2 | M1 | Nested CV on a time-grouped dataset | Keep before M3 |
| M3 | M2, M4 basics | Boosting hyperparameter reasoning, SHAP caveats | Keep before M10 |
| M4 | M1, M2 | Calibration and metric-gaming critique | Never defer (high weight) |
| M5 | M2, M4, M6 | Out-of-fold target encoding with group leakage | Never defer on backend-bridge route |
| M6 | None (standalone) | Memory-safe pairwise similarity at 1M scale | Fully skippable via test-out; engine may schedule just-in-time before M5 or M8 |
| M7 | M6 (shapes), calculus refresher | Gradient check and failure-mode debugging | May follow M9 only if learner prefers; default before M9 |
| M8 | M6, M4 (recall@k) | Multilingual token-cost study | May run in parallel with M7 |
| M9 | M7, M8 | KV-cache design review under GQA | Never skipped by AI route; placed after M7 and M8 |
| M10 | M4, M2; statistics refresher for bootstrap | Sequential testing and CUPED reasoning | Statistics lesson may be pulled forward when B5 begins |

Thresholds: mastery 80% (G-7); retake spaced >= 7 days for readiness (G-6). Open remediation flag on a prerequisite blocks readiness on the dependent module.

### LF-6 Applied labs (production-framed, one line each)

| Module | Lab |
|---|---|
| M1 | Frame a SaaS churn-prediction feature: target, prediction point, action, baseline, one-pager |
| M2 | Rescue a demand-forecast model validated with random splits on time-ordered IoT readings |
| M3 | Ticket routing for an enterprise helpdesk: TF-IDF plus logistic regression versus a prompted LLM, cost-latency-accuracy table |
| M4 | UPI-style payment fraud at 0.5 percent positives: PR curve, review-capacity threshold, bootstrap CI |
| M5 | Fix three leaky pipelines, including a SQL-training versus Python-serving skew from a FastAPI feature endpoint |
| M6 | Sessionise MQTT device events and compute rolling per-device features without leakage |
| M7 | Diagnose three recorded training logs of a churn MLP |
| M8 | Token-budget guard for a FastAPI chat endpoint and top-k cosine search over 200 support articles |
| M9 | Estimate prefill and decode behaviour and KV-cache size for a voice-agent LLM tier with supplied model dimensions |
| M10 | Plan the A/B test and offline-eval statistics (CIs, power) for a smart-reply feature; ML-versus-LLM comparison for invoice extraction |

### LF-7 Currency and quality bar

| Module | Dated stamp | Memorable visual or analogy | Top 3 misconceptions handled |
|---|---|---|---|
| M1 | Oct 2026, 12-month | "Rules, ML, LLM" ladder: climb only as high as the data justifies | ML for everything; skipping the baseline; test set used for tuning |
| M2 | Oct 2026, 12-month | Dartboard plus slider: accuracy versus consistency | More complexity always helps; regularisation only lowers training error; random split on time data |
| M3 | Oct 2026, 3-month for LLM comparison | Toolbox matrix by data shape | Boosting never overfits; trees need scaling; LLMs obsolete or always right |
| M4 | Oct 2026, 12-month | Threshold slider moving a point along ROC and PR curves | 99 percent accuracy is good; ROC-AUC fits all imbalance; default 0.5 threshold |
| M5 | Oct 2026, 12-month | "Time-travel" timeline: a read replica that is ahead of your snapshot | Scale then split; oversample then CV; retrain fixes skew |
| M6 | Oct 2026 (pandas 3.0), 3-month | Shape-alignment animation for broadcasting | `apply` is vectorised; chained assignment works (changed in pandas 3.0); merge duplication is harmless |
| M7 | Oct 2026, 12-month | Computation graph with numbers | Backprop is the learning algorithm; dropout at inference; training and inference cost alike |
| M8 | Oct 2026, 3-month | Embeddings as a lossy index key; token as a toll | Embeddings capture meaning exactly; token is a word; mix models in one index |
| M9 | Oct 2026, 3-month | Library lookup: query asks, key advertises, value delivers; KV-cache as a growing notebook | Attention is a lookup; temperature 0 is exact; the LLM remembers chats |
| M10 | Oct 2026, 12-month | Peeking trap timeline | Stop at p less than 0.05; not significant means no effect; randomise per request |

## 9. Boss compliance log

| ID | Applied |
|---|---|
| G-1 | Ownership notes row added to module table; pointers for B7, B3, B2, B5 reduced to short mentions |
| G-2 | Effort semantics sentence and per-module hours (19.5 S / 13 D plus 2.0 assessment) in header and section 2.1 |
| G-3, G-4, G-5 | Tags, level paths and bands in LF-1 and LF-2 |
| G-6, G-7 | Readiness and badge thresholds corrected (Meets 3.0 for badge, Senior bar 3.5 for readiness; 70 percent hidden tests; 80 percent mastery) |
| G-8 | Libraries re-pinned on Python 3.14; facts dated October 2026 and verified |
| G-9 | Owned tags listed; secondary tag separated |
| G-10 | Question ID scheme added; Q8.3 reframed; goal-tag and senior-bar notes in LF-2 and LF-4 (per module) and section 10 per question |
| G-11 | Sections 8 LF-1 to LF-7 |
| G-12 | Dated stamps; multipliers removed; no invented statistics |
| R-B1-1 | M7 fine-tuning paragraph at most 15 min with pointer; GPU arithmetic to B7.M8 |
| R-B1-2 | M8 trimmed to concepts, tokenisation and one ANN slide; B3.M3 and B3.M4 pointers |
| R-B1-3 | P-02 limits and P-18 runner stated; offline tokeniser licence still `[verify]` |
| R-B1-4 | M6 `A:M H:S B:N`, standalone-testable, 8 placement items |
| R-B1-5 | Statistics-for-evals lesson (1.5 h) in M10 |

## 10. Seed question goal tags and senior-bar notes (G-10)

Format: ID: `B/A/H` tag; senior-bar note (mid / senior / staff).

| ID | Tags | Senior-bar note |
|---|---|---|
| Q1.1 | B:N A:M H:S | mid: picks a model; senior: defines target, action, cost, split; staff: adds uplift, feedback loops and rollout |
| Q1.2 | B:N A:M H:S | mid: definitions; senior: places LLM pre- and post-training correctly; staff: links label cost to pipeline design |
| Q1.3 | B:N A:M H:S | mid: "start simple"; senior: quantifies lift against cost; staff: sets a baseline policy for the team |
| Q2.1 | B:N A:M H:S | mid: definitions; senior: diagnoses from curves and fixes; staff: notes limits in deep models |
| Q2.2 | B:N A:M H:M | mid: "overfitting"; senior: leakage, shift, skew investigation plan; staff: monitoring and process fix |
| Q2.3 | B:N A:M H:S | mid: sparse versus shrink; senior: scaling, CV choice, weight-decay link; staff: priors view |
| Q3.1 | B:N A:M H:S | mid: bagging versus boosting; senior: tuning, importance caveats; staff: platform-level model policy |
| Q3.2 | B:N A:M H:M | mid: "use LLM or not"; senior: cost arithmetic and hybrid routing; staff: distillation and governance plan |
| Q3.3 | B:N A:M H:S | mid: algorithm; senior: assumptions, k choice, validation; staff: scale and business use |
| Q4.1 | B:N A:M H:M | mid: P/R/F1; senior: base rate, cost threshold, CI; staff: delayed labels and monitoring |
| Q4.2 | B:N A:M H:S | mid: area under curve; senior: probabilistic reading, PR under imbalance; staff: pAUC and operating regions |
| Q4.3 | B:N A:M H:S | mid: NDCG definition; senior: bias and blind spots; staff: offline-online alignment |
| Q5.1 | B:S A:M H:M | mid: test in train; senior: taxonomy and point-in-time; staff: preventive process |
| Q5.2 | B:S A:M H:M | mid: retrain; senior: parity, logging, contract; staff: feature platform and shadow rollout |
| Q5.3 | B:N A:M H:S | mid: SMOTE; senior: fold-only resampling, calibration; staff: label and collection strategy |
| Q6.1 | B:N A:M H:S | mid: rule; senior: silent shape bug and chunking; staff: team conventions |
| Q6.2 | B:N A:M H:S | mid: loops; senior: leakage-safe windows; staff: when to move to SQL or DuckDB |
| Q6.3 | B:N A:M H:S | mid: dedupe after; senior: validate and indicator; staff: pipeline assertions |
| Q7.1 | B:N A:M H:S | mid: steps; senior: separates backprop and optimiser, debug habits; staff: training budget |
| Q7.2 | B:N A:M H:S | mid: "inference is cheaper"; senior: memory components, phases; staff: cost and serving trade-offs |
| Q7.3 | B:N A:M H:S | mid: "more layers"; senior: sanity checks and gradient flow; staff: experiment hygiene |
| Q8.1 | B:N A:M H:M | mid: cosine; senior: normalisation equivalences, versioning; staff: migration plan |
| Q8.2 | B:N A:M H:M | mid: subwords; senior: cost and failure modes; staff: multilingual cost at scale |
| Q8.3 | B:N A:S H:S | mid: "vector DB"; senior: recall-latency-memory triangle with measurement; staff: error budget and migration |
| Q9.1 | B:N A:M H:M | mid: names Q, K, V; senior: mechanics, scaling, masking, cost; staff: variants and trade-offs |
| Q9.2 | B:N A:M H:M | mid: "caches responses"; senior: formula and phases; staff: serving levers and cost |
| Q9.3 | B:N A:M H:M | mid: names; senior: task-to-architecture mapping; staff: honest boundary blur and cost |
| Q10.1 | B:N A:M H:S | mid: split traffic; senior: power, SRM, guardrails; staff: interference, long-term holdouts |
| Q10.2 | B:N A:M H:S | mid: "ship"; senior: peeking and effect size; staff: sequential design up front |
| Q10.3 | B:N A:M H:M | mid: picks a model; senior: cost model, hybrid routing, field metrics; staff: governance and vendor change |
- Track lead B review (2026-10-06): see `_reviews/TrackLead_B_Report.md`. Applied: route position and dependency note added; M6 placement count (8) stated; worked-example table added (3.1); per-lesson visuals note; M10 lessons merged to 6 (template max); M6 katas capped at 8; reference-learner hours corrected to about 15 h (study-band, not Deeper); GX [verify] given exact CI check; canvas task marked optional for badge and outside 2.0 h allowance; M9 currency (RLVR post-training, sliding-window and latent-KV awareness); open item 2 updated; last_reviewed set to 2026-10-06.
