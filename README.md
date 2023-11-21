# provastarwars
puxa uma lista de 10 personagens aleatorios de starwars
ou a mesma com peso e altura 



import React, { useEffect, useState } from 'react';
import { View, Text, FlatList, StyleSheet } from 'react-native';

const App = () => {
  const [characters, setCharacters] = useState([]);

  useEffect(() => {
    const fetchCharacters = async () => {
      try {
        const response = await fetch('https://swapi.dev/api/people/');
        const data = await response.json();
        
        // Embaralhar a lista de personagens usando Fisher-Yates Shuffle
        const shuffledCharacters = shuffleArray(data.results);
        
        // Selecionar os primeiros 10 personagens da lista embaralhada
        setCharacters(shuffledCharacters.slice(0, 10));
      } catch (error) {
        console.error('Error fetching characters:', error);
      }
    };

    fetchCharacters();
  }, []);

  // Função para embaralhar um array usando Fisher-Yates Shuffle
  const shuffleArray = (array) => {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Lista de 10 Personagens Star Wars</Text>
      <FlatList
        data={characters}
        keyExtractor={(item) => item.name}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <Text style={styles.name}>{item.name}</Text>
            <Text>Altura: {item.height}</Text>
            <Text>Peso: {item.mass}</Text>
          </View>
        )}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 16,
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    marginBottom: 16,
  },
  item: {
    marginBottom: 16,
    borderBottomWidth: 1,
    paddingBottom: 8,
  },
  name: {
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default App;
