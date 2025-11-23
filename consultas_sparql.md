# Consultas SPARQL

## 1. Ficha de Treino

```sql
PREFIX : <http://www.semanticweb.org/jonat/ontologies/2025/10/untitled-ontology-7#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT ?aluno ?exercicio ?Series ?Reps ?Descanso ?Intensidade ?Duracao
WHERE {
  # Ligações principais
  ?aluno :recebePlano ?plano .
  ?plano :compostoPor ?exercicio .

  # Dados preenchidos pelas regras
  OPTIONAL { ?exercicio :numeroSeries ?Series }
  OPTIONAL { ?exercicio :numeroRepeticoes ?Reps }
  OPTIONAL { ?exercicio :tempoDescanso ?Descanso }
  OPTIONAL { ?exercicio :duracaoRecomendada ?Duracao }
  OPTIONAL { ?exercicio :temIntensidade ?Intensidade }
}
ORDER BY ?aluno
```

## 2. Relatório de Restrições

```sql
PREFIX : <http://www.semanticweb.org/jonat/ontologies/2025/10/untitled-ontology-7#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?aluno ?restricao ?tipoDeProblema
WHERE {
  ?aluno rdf:type :Pessoa .
  ?aluno :temRestricao ?restricao .
  
  # Descobre a classe pai da restrição
  ?restricao rdf:type ?tipoDeProblema .
  ?tipoDeProblema rdfs:subClassOf :RestricaoFisica .
  
  # Filtros
  FILTER (?tipoDeProblema != :RestricaoFisica) 
  FILTER (?tipoDeProblema != :OutrasRestricoes)
}
ORDER BY ?aluno
```

## 3. Estatística por Objetivos

```sql
PREFIX : <http://www.semanticweb.org/jonat/ontologies/2025/10/untitled-ontology-7#>

SELECT ?objetivo (COUNT(?aluno) as ?QtdAlunos)
WHERE {
  ?aluno :temObjetivo ?objetivo .
}
GROUP BY ?objetivo
```
