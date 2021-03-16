# ModuleOne

void SpanningTreeBuilder::printResult(vector<Edge>& edgesList) {
    int totalWeight = 0;
    cout << "Spanning tree: " << endl;
    for (auto& edge : edgesList) {
        cout << to_string(edge.source) << " <-> " << to_string(edge.dist) <<
                    " weight: " << to_string(edge.weight) << endl;
        totalWeight += edge.weight;
    }
    cout << "Total weight of the spanning tree is: " << totalWeight;
}
