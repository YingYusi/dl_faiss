我们主要的改动有：

1.使用更强大的句子嵌入模型
model = SentenceTransformer('all-MPNet-base-v2')

2.使用余弦相似度索引
dim = data_embeddings.shape[1]
index = faiss.IndexFlatIP(dim)
index.add(data_normalized_embeddings)

3.使用循环遍历数据集
for i, idx in enumerate(I[0]):
    if i == 0:
      continue
    if i == 1:
      if D[0][i]>= 0.5: #当最相似的那个文本的余弦相似度超过了0.5，认为存在抄袭
        flag_is_plagiarism = True
        plagiarismed_sentence = data_sentences[idx]
    print(f"{i}. {data_sentences[idx]} (Similarity: {D[0][i]:.4f})")
return flag_is_plagiarism,plagiarismed_sentence

4.在抄袭定位检测中，分割了句子
sentences_a = split_sentences(query_sentence)
def split_sentences(text):
    sentences = nltk.sent_tokenize(text)
    return sentences
