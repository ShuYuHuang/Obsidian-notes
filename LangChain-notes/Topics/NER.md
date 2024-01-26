# Model products:
- Rosetta (expensive)
- netowl (only one license)

# Open source:
- [spacy](https://spacy.io): default not support Arabic?
- [xlm-roberta-ner](https://huggingface.co/Davlan/xlm-roberta-base-ner-hrl): not good at Arabic
- [**wikineural-ner**]((https://huggingface.co/Babelscape/wikineural-multilingual-ner)): good in Arabic and English 
    - Deployed, good in response time and quality
    - milisecond level inference speed
- [flan-t5-NER](https://huggingface.co/tliu/flan-t5-base-conll03-ner): Support Arabic, good, but 10~20 times slow (3s for one 200 word paragraph)