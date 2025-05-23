using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace ProjetoWagner2bim
{
    public partial class Form1 : Form
    {
        enum FormaSelecionada { Nenhuma, Linha, Retangulo, Triangulo }
        FormaSelecionada formaAtual = FormaSelecionada.Nenhuma;
        Point? ponto1 = null;
        Point? ponto2 = null;
        public Form1()
        {
            InitializeComponent();
            this.Paint += new PaintEventHandler(Form1_Paint);
            this.MouseClick += Form1_MouseClick;
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void Form1_MouseClick(object sender, MouseEventArgs e)
        {
            if (ponto1 == null)
            {
                ponto1 = e.Location;
                MessageBox.Show("Clique registrado!1" + ponto1);

            }
            else if (ponto2 == null)
            {
                MessageBox.Show("Clique registrado!2" + ponto2);
                ponto2 = e.Location;
                this.Invalidate(); 
            }
            else
            {
                ponto1 = e.Location;
                ponto2 = null;
                MessageBox.Show("Clique registrado!3 - só muda o ponto 1" + ponto1);
            }
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            if (ponto1 != null && ponto2 != null)
            {
                switch (formaAtual)
                {
                    case FormaSelecionada.Linha:
                        Reta(e, ponto1.Value.X, ponto1.Value.Y, ponto2.Value.X, ponto2.Value.Y, Color.Black);
                        break;

                    case FormaSelecionada.Retangulo:
                        DesenharRetangulo(e, ponto1.Value, ponto2.Value, Color.Red);
                        break;
                    case FormaSelecionada.Triangulo:
                        DesenharRetangulo(e, ponto1.Value, ponto2.Value, Color.Red);
                        break;
                }
            }
        }

        public void Reta(PaintEventArgs e, int x0, int y0, int x1, int y1, Color cor)
        {
            Graphics g = e.Graphics;
            Pen pen = new Pen(cor);


            int dx = Math.Abs(x1 - x0);
            int dy = -Math.Abs(y1 - y0);
            int sx = x0 < x1 ? 1 : -1;
            int sy = y0 < y1 ? 1 : -1;
            int err = dx + dy;


            while (true)
            {
                g.DrawRectangle(pen, x0, y0, 1, 1);


                if (x0 == x1 && y0 == y1) break;
                int e2 = 2 * err;
                if (e2 >= dy)
                {
                    err += dy;
                    x0 += sx;
                }
                if (e2 <= dx)
                {
                    err += dx;
                    y0 += sy;
                }
            }
        }

        private void DesenharRetangulo(PaintEventArgs e, Point p1, Point p2, Color cor)
        {
            // Ajusta para qualquer direção dos pontos
            int x = Math.Min(p1.X, p2.X);
            int y = Math.Min(p1.Y, p2.Y);
            int largura = Math.Abs(p2.X - p1.X);
            int altura = Math.Abs(p2.Y - p1.Y);

            Graphics g = e.Graphics;
            Pen pen = new Pen(cor);
            g.DrawRectangle(pen, x, y, largura, altura);
        }


        private void button1_Click(object sender, EventArgs e)
        {
            formaAtual = FormaSelecionada.Linha;
            ponto1 = null;
            ponto2 = null;
            MessageBox.Show("Modo linha ativado! Clique em dois pontos na tela.");
        }

        private void button4_Click(object sender, EventArgs e)
        {
            formaAtual = FormaSelecionada.Retangulo;
            ponto1 = null;
            ponto2 = null;
            MessageBox.Show("Modo retângulo ativado. Clique dois pontos.");
        }

        private void button3_Click(object sender, EventArgs e)
        {

        }
    }
}
