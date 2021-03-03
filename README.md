# Task3
This project is about the login and signup page but the most important thing is the database in which we develop a table name Signup_info and connect it to the project where all the enteries are being stored and an archive table where all the data of the Signup_info will copy after every 10 seconds automatically. 

# Language
ADO.NET or C# is use to develop this project by using Visual Studio.

# Classes
 Main clasees are;
 Form1.cs
 Signup.cs
 Connection.cs
 
 # Form1.cs
 using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Signup_page_Task_3
{
    public partial class login : Form
    {
        Connection obj = new Connection();

        public login()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            this.Hide();
            Signup signup = new Signup();
            signup.Show();
        }


        private void btnlogin_Click_1(object sender, EventArgs e)
        {
            if (txtpass.Text != "" && txtuname.Text != "")
            {
                if (obj.Login(txtuname.Text, txtpass.Text) == true)
                {
                    MessageBox.Show("Success!", "Login Successful!", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
                else
                {
                    MessageBox.Show("Failure!", "Login Failed!", MessageBoxButtons.OK, MessageBoxIcon.Error);
                }
            }
            else
            {
                MessageBox.Show("error!", "Validation Error!", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

       
    }
}

# Signup.cs

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Signup_page_Task_3
{
    public partial class Signup : Form
    {
        Connection obj = new Connection();
        string q, status;

        public Signup()
        {
            InitializeComponent();
        }

        private void textBox1_TextChanged(object sender, EventArgs e)
        {

        }

        private void textBox11_TextChanged(object sender, EventArgs e)
        {
            if (txtpass.Text == txtconfirmpass.Text)
            {
                label5.Text = "Password Matched";
            }
            else
            {
                label5.Text = "Password did not match";
            }
        }

        private void btnsubmit_Click(object sender, EventArgs e)
        {
            if (txtfirstname.Text!= "" && txtlastname.Text !="" && txtemail.Text !="" && comboBoxgender.Text !="" && txtconfirmpass.Text !="")
            {
                if (txtpass.Text == txtconfirmpass.Text)
                {
                    
                    q = "Insert into Signup_Info1 Values ('" + txtuname.Text + "','" + txtfirstname.Text + "','" + txtlastname.Text + "','" + txtconfirmpass.Text + "','" + comboBoxgender.Text + "','" + txtemail.Text + "','" + txtcontactno.Text + "','" + txtcnic.Text + "','" + status + "')";
                    obj.Manipulate(q);
                    MessageBox.Show("Success!", "User Added Successfully!", MessageBoxButtons.OK, MessageBoxIcon.Information);
                    this.Hide();
                    login login = new login();
                    login.Show();
                }
                else
                {
                    MessageBox.Show("Error!", "Password did not match!", MessageBoxButtons.OK, MessageBoxIcon.Error);
                }
            }
            else
            {
                MessageBox.Show("Validation Error!", "Please Enter the required fields!", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void Signup_Load(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {
            button2.BackColor = System.Drawing.Color.Gray;
            if (button1.BackColor == System.Drawing.Color.Gray)
            {
                status = "Active";
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            button1.BackColor = System.Drawing.Color.Gray;
            if (button2.BackColor == System.Drawing.Color.Gray)
            {
                status = "InActive";
            }

        }
    }
}


# Connection.cs

using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Signup_page_Task_3
{
    class Connection
    {
        SqlCommand cmd;
        SqlDataAdapter da;
        DataSet ds;
       
        static SqlConnection sc;

        public static SqlConnection GetConnection()
        {
            if (sc == null)
            {
                sc = new SqlConnection();
                sc.ConnectionString = ConfigurationManager.ConnectionStrings["App"].ToString();
                sc.Open();
            }
            return sc;
        }

        public void Manipulate(string qry)
        {
            try
            {
                cmd = new SqlCommand();
                cmd.CommandText = qry;
                cmd.Connection = Connection.GetConnection();
                cmd.ExecuteNonQuery();
            }
            catch (Exception ex)
            {
                sc.Close();
            }
        }

        public bool Login(String Username, String Password)
        {
            SqlCommand check_User = new SqlCommand("SELECT COUNT(*) FROM Signup_Info1 WHERE Username = '" + Username + "' AND Password = '" + Password + "'", Connection.GetConnection());
            check_User.Parameters.AddWithValue("Username", Username);
            check_User.Parameters.AddWithValue("Password", Password);
            int UserExist = (int)check_User.ExecuteScalar();

            if (UserExist > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
            sc.Close();
        }

    }
}

