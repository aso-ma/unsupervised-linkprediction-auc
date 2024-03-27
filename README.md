# Unsupervised Link Prediction 
A Python implementation of unsupervised link prediction problem using `NetworkX` library.


## Train Test Split

```
from scoring_function import preferential_attachment
from data_splitting import train_test_split
from evaluation import auc, precision
from link_prediction import edge_scoring
from networkx import karate_club_graph


if __name__ == '__main__':
    g_karate = karate_club_graph()
    g_train, g_test = train_test_split(g_karate)
    scored_links = edge_scoring(g_train, preferential_attachment)
    print('Precis ->', precision(g_test, scored_links))
    print('AUC ->', auc(g_karate, g_train, g_test, preferential_attachment))
```

## K-Fold Cross Validation
```
from scoring_function import preferential_attachment
from data_splitting import k_fold_cross_validation, get_train_test_graphs_for
from evaluation import auc, precision
from link_prediction import edge_scoring
from networkx import karate_club_graph


if __name__ == '__main__':
    g_karate = karate_club_graph()
    k_fold_data = k_fold_cross_validation(g_karate, 3, True)
    precision_list = []
    auc_list = []
    for fold_number, fold_edges in k_fold_data.items():
        train_graph, test_graph = get_train_test_graphs_for(g_karate, fold_edges)
        scored_links = edge_scoring(train_graph, preferential_attachment)
        precision_list.append(
            precision(test_graph, scored_links)
        )
        auc_list.append(
            auc(
                g_karate, train_graph, test_graph, preferential_attachment
            )
        )
    final_precision = sum(precision_list) / len(precision_list)
    final_auc = sum(auc_list) / len(auc_list)
    print('Precis ->', final_precision)
    print('AUC ->', final_auc)
```