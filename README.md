// File: screens/SettingsScreen.tsx
import React from 'react';
import { View, Switch } from 'react-native';
import * as Updates from 'expo-updates';
import Constants from 'expo-constants';
import { signOut } from 'firebase/auth';
import { auth } from '../App';
import { useNavigation } from '@react-navigation/native';
import Toast from 'react-native-root-toast';
import Text from '../components/Text';
import Button from '../components/Button';

const ADMIN_EMAIL = 'alinadaryab@gmail.com';

export default function SettingsScreen() {
  const navigation = useNavigation();
  const [offline, setOffline] = React.useState(true);
  const user = auth.currentUser;
  const isAdmin = user?.email === ADMIN_EMAIL;

  const handleLogout = async () => {
    try {
      await signOut(auth);
      Toast.show('Logged out', { duration: Toast.durations.SHORT });
      navigation.navigate('Login');
    } catch {
      Toast.show('Logout failed', { duration: Toast.durations.SHORT });
    }
  };

  return (
    <View className="flex-1 bg-white p-6">
      <Text variant="heading" style={{ marginBottom: 20 }}>Settings</Text>

      <View className="flex-row items-center justify-between mb-6">
        <Text variant="body">Offline Mode</Text>
        <Switch value={offline} onValueChange={setOffline} />
      </View>

      <Text variant="label" className="mb-8">
        App Version: {Constants.expoConfig?.version || '1.0.0'}
      </Text>

      {isAdmin && (
        <>
          <Text variant="subheading" className="mb-2 mt-2">Developer Tools</Text>
          <Button
            title="View Analytics"
            onPress={() => navigation.navigate('AdminPIN', { redirectTo: 'Analytics' })}
            style={{ marginBottom: 12 }}
          />
          <Button
            title="Manage Experiences"
            onPress={() => navigation.navigate('AdminPIN', { redirectTo: 'AdminExperienceManager' })}
            style={{ marginBottom: 12 }}
          />
        </>
      )}

      <Button title="Logout" onPress={handleLogout} />
    </View>
  );
}
