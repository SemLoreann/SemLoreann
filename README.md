CMD:checkoff(playerid, params[])
{
	static name[MAX_PLAYER_NAME];
	if(sscanf(params,"s[25]", name)) return SendClientMessage(playerid, c_ARGON, "Используйте: /checkoff [Ник]");
	
	static fmt_str[] = "SELECT * FROM "TABLE_ACCOUNT" WHERE `Name` = '%e'";
	new string[sizeof(fmt_str) + MAX_PLAYER_NAME];
	mysql_format(MySQL, string, sizeof(string), fmt_str, name);
	mysql_query(MySQL, string, true);
  
	if(cache_num_rows())
    {
		new cash, bank;

		cash = cache_get_value_name_int(name, "Cash", PlayerInfo[playerid][pCash]);
		bank = cache_get_value_name_int(name, "Bank", PlayerInfo[playerid][pBank]);

		static const fmt_str_1[] = "Ник: %s\nНаличные: %i$\nБанковский счёт: %i$";
		new string_1[sizeof(fmt_str_1) + MAX_PLAYER_NAME + 11 + 11 + 10];
		format(string_1, sizeof(string_1), fmt_str_1, name, cash, bank);
		ShowPlayerDialog(playerid, D_NULL, 0, "Cтатистика", string_1, "Мне пора","");
		return true;
	}
	else return SendClientMessage(playerid, c_ARGON, "Аккаунт не найден.");
}
