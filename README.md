import React, { useState, useRef } from 'react';
import { Text, StyleSheet, View, Image, RefreshControl, FlatList, Animated, ImageBackground } from 'react-native';

const data = [
  { id: '1', name: 'Nguyễn Ngọc Lợi', mssv: 'B1907738', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
  { id: '2', name: 'Trần Trọng Duy', mssv: 'B1907782', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
  { id: '3', name: 'Huỳnh Phúc Vinh', mssv: 'B1907831', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
];

const additionalData = [
  { id: '4', name: 'Nguyễn Minh Thiện', mssv: 'B1907818', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
  { id: '5', name: 'Lê Hữu Nhân', mssv: 'B1907747', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
  { id: '6', name: 'Phạm Hữu Đấu', mssv: 'B1907728', image: 'https://upload.wikimedia.org/wikipedia/vi/thumb/6/6c/Logo_Dai_hoc_Can_Tho.svg/2048px-Logo_Dai_hoc_Can_Tho.svg.png' },
];

const ItemSeparator = () => {
  return (
    <View style={styles.separator} />
  );
};

const MyFlatList = () => {
  const [refreshing, setRefreshing] = useState(false);
  const [shuffledData, setShuffledData] = useState(data);
  const [reachedEnd, setReachedEnd] = useState(false);
  const animatedScale = useRef(new Animated.Value(1)).current;

  const shuffleArray = (array) => {
    let shuffled = array.slice();
    for (let i = shuffled.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
    }
    return shuffled;
  };

  const onRefresh = () => {
    // Xáo trộn dữ liệu
    const shuffled = shuffleArray(shuffledData);
    setRefreshing(true);
    setTimeout(() => {
      setRefreshing(false);
      setShuffledData(shuffled);
      animatedScale.setValue(0);
      Animated.timing(animatedScale, {
        toValue: 1,
        duration: 1000,
        useNativeDriver: false,
      }).start();
    }, 2000);
  };

  const loadMoreData = () => {
    if (!reachedEnd) {
      // Nạp thêm dữ liệu ở đây, ví dụ: setReachedEnd(true) để ngăn người dùng nạp thêm nữa
    }
  };

  const renderItem = ({ item, index }) => (
    <Animated.View style={[styles.item, { transform: [{ scale: index === 0 ? animatedScale : 1 }] }]}>
      <Image source={{ uri: item.image }} style={styles.itemImage} />
      <View style={styles.itemTextContainer}>
        <Text style={styles.itemText}>{item.name}</Text>
        <Text style={styles.itemMssv}>{item.mssv}</Text>
      </View>
    </Animated.View>
  );

  return (
    <View style={styles.container}>
      <View style={styles.headerContainer}>
        <Text style={styles.header}>Nhóm 07 _KC359</Text>
      </View>
      <FlatList
        style={styles.flatList}
        data={shuffledData.concat(additionalData)}
        renderItem={renderItem}
        keyExtractor={(item) => item.id}
        refreshControl={
          <RefreshControl
            refreshing={refreshing}
            onRefresh={onRefresh}
            tintColor="black"
            title="Đang làm mới..."
            titleColor="black"
          />
        }
        ItemSeparatorComponent={ItemSeparator} // Sử dụng ItemSeparatorComponent
        onEndReached={loadMoreData} // Xử lý khi người dùng cuộn đến cuối danh sách
        onEndReachedThreshold={0.1} // Khoảng cách từ cuối danh sách để gọi loadMoreData (0.1 nghĩa là 10%)
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'white',
    marginTop: 20,
  },
  flatList: {
    flex: 1,
  },
  item: {
    backgroundColor: '#f9c2ff',
    padding: 30,
    width: '100%',
    flexDirection: 'row',
    alignItems: 'center',
  },
  separator: {
    height: 2,
    backgroundColor: 'black', // Màu của phần tách
  },
  itemImage: {
    width: 60,
    height: 60,
    marginRight: 10,
  },
  itemTextContainer: {
    flex: 1,
    flexDirection: 'column',
  },
  itemText: {
    color: 'black',
    fontSize: 30,
    fontWeight: 'bold',
  },
  itemMssv: {
    color: 'black',
    fontSize: 25,
  },
  headerContainer: {
    backgroundColor: 'blue',
    width: '100%',
    paddingVertical: 20,
    justifyContent: 'center',
    alignItems: 'center',
  },
  header: {
    fontSize: 30,
    fontWeight: 'bold',
    color: 'white',
  },
});

export default MyFlatList;
