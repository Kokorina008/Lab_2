``` Java
import java.util.Scanner;
import java.io.PrintStream;
public class Main {
    public static Scanner in = new Scanner(System.in);
    public static PrintStream out = System.out;
    static boolean isProbel(char c) { //является ли символ пробелом?
        return c==' ';
    }
    
    static boolean start(String word) {//начинается ли с заглавной буквы?
        if (word.length() == 0) return false;
        char c = word.charAt(0);
        return ((c>='A' && c<='Z') || (c>='А' && c<='Я'));
    }
    
    static int countWords(String s) { //Количество слов в строке
        int k = 0;
        boolean inWord = false;
        for (int i = 0; i<s.length(); i++) {
            char a = s.charAt(i);
            if (isProbel(a)==false) {
                if (inWord == false) {
                    k++;
                    inWord = true;
                }
            } else {
                inWord = false;
            }
        }
        return k;
    }
    
    static String[] words(String s) { //Разбиение строки на слова, заполнение массива словами 
        int k = countWords(s);
        String[] news = new String[k];
        int ind = 0;
        int start = -1;
        for (int i = 0; i<s.length(); i++) {
            char a = s.charAt(i);
            if (isProbel(a) == false) {
                if (start == -1) {
                    start = i;
                }
            } else {
                if (start!=-1) {
                    news[ind] = s.substring(start,i);
                    ind++;
                    start = -1;
                }
            }
        }
        if (start!=-1) {
            news[ind] = s.substring(start, s.length());
        }
        return news;
    }
    
    static String reverse(String s) { //Разворот строки
        int len = s.length();
        char [] a = new char[len];
        for (int i = 0; i<len; i++) {
            a[i] = s.charAt(len-i-1);
        }
        return new String(a);
    }
    
    static String delete(String s) { //Исключение повторяющихся элементов
        StringBuilder news = new StringBuilder();
        for (int i = 0; i<s.length(); i++) {
            char a = s.charAt(i);
            boolean simb = false;
            for (int j = 0; j<news.length(); j++) {
                if (news.charAt(j) == a) {
                    simb = true;
                    break;
                }
            }
            if (simb == false) {
                news.append(a);
            } 
        }
        return news.toString();
    }
    
    static String replace(String s) { //Замена пробелов на -
        char [] news = new char[s.length()];
        for (int i = 0; i<s.length(); i++) {
            char a = s.charAt(i);
            if (isProbel(a) == true) {
                news[i] = '-';
            } else {
                news[i] = a;
            }
        }
        return new String(news);
    }
    
    static void mat(String[][] a) {
        for (int i = 0; i<a.length; i++) {
            for (int j = 0; j<a[i].length;j++) {
                out.print("[" + a[i][j] + "] ");
            }
            out.println();
        }
    }
    
    
    public static void main(String[] args) {//Ввод размеров массива
        out.print("Введите количество строк");
        int N = in.nextInt();
        out.print("Введите количество столбцов");
        int M = in.nextInt();
        in.nextLine();
        String[][] a = new String[N][M];
        out.println("Введите элементы массива");//Заполнение массива
        for (int i = 0; i<N; i++) {
            for ( int j = 0; j<M; j++) {
                a[i][j] = in.nextLine();
            }
        }
        
        int [] sumLen = new int[M];
        int [] sumWords = new int[M];
        for (int j = 0; j<M; j++) {
            for (int i = 0; i<N; i++) {
                sumLen[j] += a[i][j].length();
                sumWords[j] += countWords(a[i][j]);
            }
        }
        
        int [] b = new int [M];
        for (int j = 0; j<M; j++) {
            b[j] = j;
        }
        
        for (int i = 0; i < M-1; i++) {
            for (int j = 0; j < M-1-i; j++) {
                int c1 = b[j];
                int c2 = b[j+1];
                boolean x = false;
                if (sumLen[c1] < sumLen[c2]) {
                    x = true;
                } else if (sumLen[c1] == sumLen[c2] && sumWords[c1]<sumWords[c2]) {
                    x = true;
                }
                if (x == true) {
                    int z = b[j];
                    b[j] = b[j+1];
                    b[j+1] = z;
                }
            }
        }
        
        String[][] sort = new String[N][M];
        for (int i = 0; i<N; i++) {
            for (int j = 0; j<M; j++) {
                sort[i][j] = a[i][b[j]];
            }
        }
        a = sort;
        out.println("После сортировки столбцов");
        mat(a);
        
        int k = 0;
        StringBuilder s = new StringBuilder();
        for (int i = 0; i<N; i++) {
            for (int j = 0; j<M; j++) {
                String [] w = words(a[i][j]);
                for (int l = 0; l<w.length; l++) {
                    if (start(w[l])==true) {
                        if (k>0) {
                            s.append(" ");
                        }
                        s.append(w[l]);
                        k++;
                    }
                }
            }
        }
        out.println("Слова с заглавной буквы");
        out.println(k);
        out.println(s);
        
        out.println("Развернутые строки *");
        for (int i = 0; i<N; i++) {
            for (int j = 0; j<M; j++) {
                String s = a[i][j];
                String rev = reverse(s);
                
                boolean pal = true;
                for(int z = 0; z<s.length(); z++) {
                    if (s.charAt(z)!=rev.charAt(z)) {
                        pal = false;
                        break;
                    }
                }
                
                if (pal == true) {
                    out.print(rev+"* ");
                } else {
                    out.print(rev+" ");
                }
            }
            out.println();
        }
        
        out.println("Замена пробелов и удаление повторов");
        String[][] del = new String[N][M];
        for (int i = 0; i<N; i++) {
            for (int j = 0; j<M; j++) {
                String s = replace(a[i][j]);
                del[i][j] = delete(s);
            }
        }
        mat(del);
    }
    
}
```
