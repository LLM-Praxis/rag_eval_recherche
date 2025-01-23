### N-gram und Embedding basierte Generator Evaluation
Es gibt verschiedene Ansätze, um die Antwort eines RAG-Systems mit einer Ground Truth zu vergleichen und deren Ähnlichkeit zu bewerten. Die einfachste und rechnerisch effizienteste Methode besteht darin, textbasierte Vergleichsmetriken wie [BLEU](https://dl.acm.org/doi/10.3115/1073083.1073135) oder [ROUGE](https://arxiv.org/abs/1803.01937) zu verwenden. Diese Ansätze bewerten die Ähnlichkeit der beiden Texte auf Grundlage ihrer n-Gramme und textuellen Übereinstimmungen.  
Mehrere Studien und Erfahrungsberichte zeigen jedoch, dass diese Metriken insbesondere bei längeren Antworten oder solchen, die komplexes Reasoning erfordern, oft nicht mit menschlichen Präferenzen übereinstimmen. Aus diesem Grund wird von ihrer Verwendung bei RAG Evaluation in den meisten Fällen abgeraten.

Ein weiterer Ansatz ist die semantische Ähnlichkeitsbewertung, bei der die Bedeutung der Texte anstelle ihrer rein textuellen Übereinstimmung verglichen wird.
Methoden wie [BERTScore](https://arxiv.org/abs/1904.09675) oder [MoverScore](https://arxiv.org/abs/1909.02622) verwenden hierbei vortrainierte Sprachmodelle, um den semantischen Gehalt der Texte zu analysieren und die Ähnlichkeit auf einer tieferen Ebene zu bewerten. Diese Ansätze berücksichtigen Synonyme, Paraphrasen und den kontextuellen Zusammenhang der Inhalte, was zu präziseren Bewertungen führt.
Ein wesentlicher Vorteil dieser Methode ist, dass lokal gehostete Embedding-Modelle verwendet werden können, wodurch keine API-Kosten anfallen. Allerdings übertreffen Ansätze wie LLM-as-a-Judge in der Regel die Leistung dieser Methoden, was jedoch mit wesentlich höheren Kosten verbunden ist.


### LLM-basierte Evaluations Ansätze (mit Github Repos)
#### [RAGAS: Automated Evaluation of Retrieval Augmented Generation](https://aclanthology.org/2024.eacl-demo.16/) (Mär 24)
Das Paper **RAGAS** stellt ein Framework vor, das **Large Language Models (LLMs)** als Bewertungsinstanzen nutzt, um sowohl die Retrieval- als auch die Generationskomponenten von RAG-Systemen zu evaluieren. Eine Besonderheit von RAGAS ist, dass **keine Ground-Truth-Daten** benötigt werden, sondern lediglich eine Liste von Anfragen (Queries), was die Evaluierung erleichtert.  
Allerdings wurde die Effektivität von RAGAS in späteren Arbeiten hinterfragt. Unter anderem haben sowohl **ARES** als auch **RAGChecker** haben in ihren eigenen Benchmarks gezeigt, dass sie bessere Ergebnisse erzielen können.

#### [ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems](https://arxiv.org/abs/2311.09476) (Mär 24)
ARES ist ein Framework ([GitHub-Projekt](https://github.com/stanford-futuredata/ARES)), mit dem RAG-Systeme automatisiert evaluiert werden können. 
Der automatisierte Prozess kombiniert die Generierung synthetischer Daten mit feinabgestimmten Klassifikatoren, um Context Relevance, Answer Faithfulness und Answer Relevance effizient zu bewerten und dabei den Bedarf an umfangreichen menschlichen Annotationen zu minimieren. ARES nutzt synthetische Anfragengenerierung und Prediction-Powered Inference (PPI), um genaue Bewertungen mit statistischer Zuverlässigkeit zu ermöglichen.

#### [RAGChecker: A Fine-grained Framework for Diagnosing Retrieval-Augmented Generation](https://www.semanticscholar.org/paper/f08cd81718a2002516476c38255c66de1916e85d) (Aug 24)
Ein besonders interessantes und relevantes Paper, das zu folgendem GitHub-Repository gehört:
[GitHub-Projekt](https://github.com/amazon-science/RAGChecker)  
Durch die gezielte Extraktion von Claims aus den Ground Truths und zugehörigen Fragen können sowohl die Retrieval- als auch die Generation-Module präziser bewertet werden. Dadurch wurden bessere Ergebnisse als mit RAGAS, ARES und anderen erzielt.  
Es gibt eine Integration in LLAMAINDEX, wobei die Lösung unabhängig vom verwendeten RAG-Framework sehr nutzerfreundlich wirkt.

#### [SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models](https://arxiv.org/abs/2303.08896) (Mär 23)
Das Paper präsentiert einen einfachen Ansatz, um die Tendenz von LLM-Systemen zu Halluzinationen zu bewerten, der auch auf RAG-Systeme anwendbar ist. Dabei wird dieselbe Query mehrfach an dasselbe RAG-System gestellt und die Ähnlichkeit der Antworten analysiert. Große Abweichungen zwischen den Antworten deuten auf eine hohe Wahrscheinlichkeit von Halluzinationen hin. Ein entscheidender Vorteil dieses Verfahrens besteht darin, dass keine Referenzantworten benötigt werden. Die Ähnlichkeit kann unter anderem sowohl mit NLI Modellen, als auch mit LLMs ermittelt werden. [Github Link](https://github.com/potsawee/selfcheckgpt)

### Evaluation Toolboxes
Es gibt verschiedene Frameworks zur Erstellung von RAG-Systemen, wie zum Beispiel  [LangChain](https://www.langchain.com)+[LangSmith](https://www.langchain.com/langsmith), [LlamaIndex](https://www.llamaindex.ai) oder [Haystack](https://haystack.deepset.ai), welche jeweils von Grund auf Evaluationsmöglichkeiten bereitstellen. Die nachfolgend aufgeführten Frameworks bieten jedoch spezialisierte Funktionalitäten, mit denen RAG-Systeme noch effizienter getestet und optimiert werden können:
#### [TruLens](https://github.com/truera/trulens)
TruLens ist eine kostenlose Open-Source-Bibliothek, die speziell darauf ausgelegt ist, LLM-as-a-judge-Evaluierungen zu erleichtern.  
Persönliche Einschätzung: Im Vergleich zu den Toolboxes von DeepEval und RAGAS bietet TruLens einen etwas begrenzten Funktionsumfang. Beispielsweise gibt es nur wenige unterstützende Funktionen für Monitoring, Tracing, Backtesting und eine eingeschränkte Vielfalt an out-of-the-box Evaluationsmöglichkeiten. Verfügt über Integrationen zu LangChain und LlamaIndex.

#### [RAGAS](https://github.com/truera/trulens)
Basierend auf dem RAGAS-Paper wurde eine umfassende RAG-Evaluierungstoolbox entwickelt, die über die im ursprünglichen Paper beschriebenen Metriken weit hinausgeht.
Der Funktionsumfang ist ähnlich zu dem von DeepEval, mit dem Unterschied, dass diese Features nicht kostenpflichtig sind.  
RAGAS bietet eine Integration zu LlamaIndex und LangChain an und Haystack eine Integration zu RAGAS.

#### [DeepEval](https://github.com/confident-ai/deepeval)
DeepEval ist eine Open-Source-Bibliothek, die für die Evaluation von LLM- und RAG-Systemen entwickelt wurde. Sie bietet eine Vielzahl von Evaluationsmöglichkeiten und zeichnet sich durch den Einsatz von Pytest aus, was das Evaluieren auf Unit-Test Ebene besonders komfortabel macht. 
Es werden außerdem viele weitere Features wie Tracing, Caching, Parallel Testing, Data Synthesizing und Monitoring bereitsgestellt, wobei sich einige dieser Features in der kostenpflichtigen Pro-Version freischalten lassen.  
Zusätzlich bietet die Bibliothek eine breite Palette an Funktionen wie Tracing, Caching, Parallel Testing, Data Synthesizing und Monitoring. Einige dieser erweiterten Funktionen sind jedoch erst in der kostenpflichtigen Pro-Version zugänglich.  
DeepEval bietet eine Integration zu LlamaIndex an und Haystack eine Integration zu DeepEval.

#### [Phoenix - Arize AI](https://github.com/Arize-ai/phoenix)
Phoenix ist eine Open-Source-Plattform zur Beobachtung von KI-Systemen, die speziell für Experimente, Evaluierungen und Fehlersuche entwickelt wurde. 
Neben Tracing, der Erstellung von synthetischen Datasets und weiteren Features, wird auch die Evaluation von RAG Systemen ermöglicht. 
Der Funktionsumfang bezogen auf Evaluirung ist auch hier geringer, als bei RAGAS und DeepEval, in Kombination mit der angebotenen Tracing Funktionalität könnte es aber dennoch eine attraktive Lösung sein.  
Auch hier gibt es Integrationen zu LangChain, Llamaindex und Haystack.

### Finetuned Judges
In Evaluation-Frameworks wie TruLens oder RAGAS werden häufig generalisierte LLMs wie GPT-3.5 oder GPT-4 (z. B. GPT-4-turbo) als “Judge” eingesetzt. Ein alternativer, vielversprechender Ansatz besteht jedoch darin, das Bewertungsmodell gezielt auf die Evaluationsaufgabe zu fine-tunen. Dadurch können kleinere, spezialisierte Modelle eingesetzt werden, was beispielsweise zu einer Reduzierung der Kosten und des Ressourcenverbrauchs führt, ohne dabei die Genauigkeit der Evaluation zu kompromittieren.

Ein Beispiel dafür ist die Verwendung der Modelle aus dem [Prometheus-Eval](https://github.com/prometheus-eval/prometheus-eval) Framework. Darüber hinaus demonstriert das Paper [RAGBench](https://arxiv.org/abs/2407.11005), wie ein feinabgestimmtes DeBERTa-Modell als “Judge” vielversprechende Ergebnisse erzielt hat.


### Benchmarking
Benchmarks für RAG-Systeme sind speziell entwickelte Datensätze, die dazu dienen, verschiedene Eigenschaften und Fähigkeiten dieser Systeme zu bewerten. Sie sind wertvoll, um Stärken und Schwächen von RAG-Modellen offenzulegen und um den Vergleich zwischen unterschiedlichen Systemen zu ermöglichen. Allerdings eignen sich solche Benchmarks in der Regel nicht direkt zur Optimierung eines RAG-Systems für spezifische in-domain Aufgaben. Der Grund dafür liegt in der oft begrenzten Übertragbarkeit von Erkenntnissen über die Performance in out-of-domain Benchmarks auf die in-domain Anforderungen eines Systems.

Die folgenden Benchmarks bewerten verschiedene LLMs auf verschiedene Eigenschaften im Kontext von RAG:
* [Benchmarking Large Language Models in Retrieval-Augmented Generation](https://arxiv.org/abs/2309.01431) (Sep 23)  
  Das Paper untersucht die Fähigkeiten von LLMs im Kontext von RAG in den folgenden Bereichen: Noise Robustness, Negative Rejection, Information Integration und Counterfactual Robustness.
  Hierbei wird ein Benchmark vorgestellt, der speziell dazu dient, diese Eigenschaften der Modelle zu evaluieren.  
* [RECALL: A Benchmark for LLMs Robustness against External Counterfactual Knowledge](https://arxiv.org/abs/2311.08147) (Nov 23)  
  Dieses Paper stellt einen Benchmark vor, der manuell bearbeitete kontrafaktische Kontexte in QA- und Textgenerierungsdatensätzen integriert, um die kontrafaktische Robustheit von LLMs zu bewerten.
* ["Knowing When You Don't Know": A Multilingual Relevance Assessment Dataset for Robust Retrieval-Augmented Generation](https://arxiv.org/abs/2312.11361) (Dez 23)   
 
Mit den folgenden Benchmarks lassen sich RAG-Systeme ganzheitlich evaluieren:
* [RAGBench: Explainable Benchmark for Retrieval-Augmented Generation Systems](https://arxiv.org/abs/2407.11005) (Jun 24)  
RAGBench ist ein besonders umfangreicher Benchmark, der eine Sammlung von 12 Datensätzen mit insgesamt 100.000 Samples umfasst. Durch diese Vielfalt deckt er unterschiedliche Domänen und Herausforderungen ab und bietet damit eine breite Grundlage für die Bewertung von Systemen.
* ...
* TODO AttributionBench, RAGTruth
