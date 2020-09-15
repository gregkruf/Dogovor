using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Configuration;

namespace Dogovor
{
    public partial class Form_Dogovor : Form
    {
        private SqlConnection sqlConnection = null;
        public Form_Dogovor()
        {
            InitializeComponent();
        }

        private async void Form1_Load(object sender, EventArgs e)
        {
            string connectionString = @"Data Source=(LocalDB)\v11.0;AttachDbFilename=D:\Sangrinik\Dogovor\Dogovor\Dogovora.mdf;Integrated Security=True";
            
            sqlConnection = new SqlConnection(connectionString);

            await sqlConnection.OpenAsync();

            listView1.GridLines = true;
            listView1.FullRowSelect = true;
            listView1.View = View.Details;

            
            ContextMenuStrip contextMenu = new ContextMenuStrip();
            listView1.ContextMenuStrip = ContextMenu;
            

            listView1.Columns.Add("Номер в базе");
            listView1.Columns.Add("Номер договора");
            listView1.Columns.Add("Дата создания");
            listView1.Columns.Add("ID контрагента");
            listView1.Columns.Add("Дата исполнения");
            listView1.Columns.Add("Дата окончания договора");
            listView1.Columns.Add("Сумма по договору");
            listView1.Columns.Add("Платеж");
            listView1.Columns.Add("ID Пункта по 44ФЗ");



        }

        private void Form_Dogovor_FormClosing(object sender, FormClosingEventArgs e)
        {
            if (sqlConnection != null && sqlConnection.State != ConnectionState.Closed)
                sqlConnection.Close();
        }

        private async Task LoadContractsAsync() //Select
        {
            SqlDataReader sqlReader = null;

            SqlCommand getContractsCommand = new SqlCommand("Select * From [Contracts]", sqlConnection);

            try
            {
                listView1.Items.Clear();
                sqlReader = await getContractsCommand.ExecuteReaderAsync();

                while (await sqlReader.ReadAsync())
                {
                    ListViewItem item = new ListViewItem(new string[]{
                        Convert.ToString(sqlReader["Id"]),
                        Convert.ToString(sqlReader["Number_Contract"]),
                        Convert.ToString(sqlReader["Date_Contr"]),
                        Convert.ToString(sqlReader["Id_Partner"]),
                        Convert.ToString(sqlReader["Date_Execution"]),
                        Convert.ToString(sqlReader["Date_Finishing"]),
                        Convert.ToString(sqlReader["Sum_Money"]),
                        Convert.ToString(sqlReader["Payment"]),
                        Convert.ToString(sqlReader["Id_Point_44FZ"])
                    });

                    listView1.Items.Add(item);
                    
                }

            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
            finally
            {
                if (sqlReader != null && !sqlReader.IsClosed)
                {
                    sqlReader.Close();
                }
            }
        }

        private async void buttonSelectContracts(object sender, EventArgs e)
        {
            await LoadContractsAsync();
        }

        private void buttonInsertContracts(object sender, EventArgs e)
        {
            INSERT_Contracts insert = new INSERT_Contracts(sqlConnection);

            insert.Show();
        }
    }
}
