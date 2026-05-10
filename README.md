# Inférence automatique de relations entre phrases en français  
---

# Contexte général

## Natural Language Inference (NLI)

La **reconnaissance d’inférence textuelle (NLI)** est une tâche centrale en traitement automatique des langues (TAL).  
Elle consiste à déterminer la relation logique entre deux phrases :

-  **Conséquence (Entailment)**  
-  **Contradiction**  
-  **Neutre**

Ce projet s’inscrit dans le cadre du cours **Representation Learning for NLP** et vise à comparer différentes approches modernes de modélisation linguistique en français.

---

# Objectifs du projet

Ce travail poursuit trois objectifs pédagogiques majeurs :

## 1. Comprendre CamemBERT vs CamemBERTa

- **:contentReference[oaicite:0]{index=0}**
  - Basé sur RoBERTa
  - Pré-entraîné uniquement sur corpus français (OSCAR)
  - Spécialisé monolingue

- **:contentReference[oaicite:1]{index=1}**
  - Basé sur mBART
  - Pré-entraînement multilingue (25 langues)
  - Architecture de type denoising seq2seq

 Objectif : comparer **spécialisation monolingue vs généralisation multilingue** sur la tâche NLI en français.

---

## 2. Maîtriser LoRA (Low-Rank Adaptation)

LoRA est une technique de **fine-tuning efficace (PEFT)**.

### Principe théorique

Pour une matrice de poids \( W \), LoRA approxime la mise à jour :

\[
W = W_0 + BA
\]

avec :
- \( B \in \mathbb{R}^{d \times r} \)
- \( A \in \mathbb{R}^{r \times k} \)
- \( r \ll \min(d,k) \)

### Intérêts

-  Réduction massive des paramètres entraînables
-  Préservation des connaissances du modèle
-  Modularité des adaptations

 Permet d’adapter des modèles LLM avec très peu de ressources.

---

## 3. Apprentissage en contexte et Chain-of-Thought

###  In-context learning (0-shot / few-shot)
Introduit par **:contentReference[oaicite:2]{index=2} et al. (GPT-3)** :

- Apprentissage sans mise à jour des poids
- Utilisation d’exemples dans le prompt

###  Chain-of-Thought (CoT)

- Introduit par **:contentReference[oaicite:3]{index=3}**
- Génère un raisonnement intermédiaire explicite

 Avantages :
- Meilleure interprétabilité
- Résolution de tâches multi-étapes
- Meilleure structure logique

---

#  Données

Le dataset contient :

-  Train : 5010 exemples  
-  Test : 2490 exemples  

Chaque exemple contient :
- une prémisse
- une hypothèse
- un label (entailment / contradiction / neutral)

---

#  Méthodologie expérimentale

## 1. Fine-tuning supervisé avec LoRA

- CamemBERT + LoRA
- CamemBERTa + LoRA
- LLaMA 3.2 1B + LoRA
- LLaMA 3.2 3B + LoRA

---

## 2. Apprentissage en contexte (:contentReference[oaicite:4]{index=4} 3.2 3B)

-  0-shot : sans exemples  
-  Few-shot : 2–3 exemples par classe  
-  Chain-of-Thought : raisonnement explicite  

---

##  Résultats expérimentaux

| Modèle | Méthode | Accuracy |
|--------|--------|----------|
| CamemBERT | LoRA | 71.66 % |
| CamemBERTa | LoRA | 72.65 % |
| LLaMA 1B | LoRA | 72.60 % |
| LLaMA 3B | LoRA | **82.80 %** |
| LLaMA 3B | 0-shot | 33.5 % |
| LLaMA 3B | Few-shot | 36.0 % |
| LLaMA 3B | Chain-of-Thought | ~41.3 % |

---

#  Observations principales

##  1. Efficacité de LoRA

- < 1% des paramètres entraînés
- Performances élevées (72–83%)

---

##  2. Meilleur modèle

 **LLaMA 3.2 3B + LoRA**
- 82.8% accuracy
- Meilleur compromis performance / coût

---

##  3. Apprentissage en contexte

- 0-shot : très faible performance
- Few-shot : amélioration limitée
- CoT : meilleure interprétabilité mais pas forcément meilleure précision

---

##  4. CamemBERT vs CamemBERTa

- Gain faible (~+0.99%)
- Avantage multilingue limité pour NLI français

---

##  5. Difficultés observées

- Inférences causales implicites difficiles
- Classe Neutre souvent confondue
- Négation et quantificateurs logiques problématiques

---

#  Analyse pédagogique

##  CamemBERT vs CamemBERTa

- Monolingue déjà très performant
- Multilingue utile surtout pour transfert cross-lingue

---

##  LoRA

- Solution efficace pour fine-tuning à faible coût
- Permet d’adapter des LLM massifs facilement

---

##  Prompting

- LLMs sensibles au format du prompt
- Raisonnement pas toujours fiable sans fine-tuning

---

#  Conclusion

La meilleure approche pour la NLI en français est :

>  **LLaMA 3.2 3B + LoRA**

### Pourquoi ?
- Meilleure performance globale (82.8%)
- Bonne généralisation
- Fine-tuning léger mais efficace

---

#  Perspectives

- Instruction tuning avancé
- Prompt engineering structuré (Program-of-Thought)
- Extension multilingue
- Analyse plus fine du raisonnement LLM

---

#  Technologies utilisées

- PyTorch
- Hugging Face Transformers
- Datasets
- PEFT (LoRA)
- TRL (SFTTrainer)
- Scikit-learn
- NumPy
