using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

namespace работа_с_БД
{
    class Program
    {
        static void Main(string[] args)
        {
            string connectionStr = "server=localhost;user=root;database=sgndb;password=Piramida-10;";
            MySqlConnection conn = new MySqlConnection(connectionStr);

            conn.Open();
            string sql = "Select names from users where id = 4";

            MySqlCommand command = new MySqlCommand(sql, conn);

            string name = command.ExecuteScalar().ToString();
            Console.WriteLine(name);
            Console.WriteLine("-----");

            string sql1 = "Select id, names from users where age = 22";
            MySqlCommand command1 = new MySqlCommand(sql1, conn);

            MySqlDataReader reader = command1.ExecuteReader();
            while(reader.Read())
            {
                Console.WriteLine(reader[0].ToString() + "  " + reader[1].ToString());

            }
            reader.Close();



            conn.Close();

            string connectionStr = "server=localhost;user=root;database=sgndb;password=Piramida-10;";
            MySqlConnection conn = new MySqlConnection(connectionStr);

            conn.Open();
            string querry = "insert into users (id, names, sign, date, age) values (6, 'tom', 'ttk', '2000-01-01', 20)";

            MySqlCommand command = new MySqlCommand(querry, conn);
            command.ExecuteNonQuery();


            conn.Close();



        }
    }
}
