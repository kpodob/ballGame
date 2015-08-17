# ballGame
package BallGame;

import java.util.concurrent.ExecutorService;

import static java.util.concurrent.Executors.newCachedThreadPool;

/**
 * Created by AndrzejKr�l on 2015-08-06.
 */
public class Ball{
    public int X = 4;
    public int Y = 4;
    private int wynik = 0;
    Plansza plansza;
    ExecutorService cachExec = newCachedThreadPool();

    public Ball(Plansza plansza){
        this.plansza = plansza;
        plansza.getPole(this.X, this.Y).setJestNaNimPilkaBadzNie(true);
    }

    public int getWynik() {
        return wynik;
    }

    public void setWynik(int wynik) {
        this.wynik = wynik;
    }

    public void setY(int y) {
        Y = y;
    }

    public void setX(int x) {
        X = x;
    }

    public int getX() {
        return X;
    }

    public int getY() {
        return Y;
    }

    public void wGore(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getGora();
            if(temp.isZajeteBadzNie() == false){
                if(temp.getObiektJedzony() != null){
                    temp.setJestNaNimPilkaBadzNie(true);
                    cachExec.execute(plansza.getPole(X, Y));
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    temp.getObiektJedzony().setZjedzonyBadzNie(true);
                    Y = Y + 1;
                    wynik = wynik + 1;
                }
                else {
                    plansza.getPole(X, Y).setZajeteBadzNie(false);
                    temp.setJestNaNimPilkaBadzNie(true);
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    cachExec.execute(plansza.getPole(X, Y));
                    Y = Y + 1;
                }
            }
            else {
                System.out.println("Pole jest zablokowane, badz koniec planszy!");
            }
        }
        catch (NullPointerException e){
            System.out.println("Pole jest zablokowane, badz koniec planszy!");
        }
    }

    public void wDol(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getDol();
            if(temp.isZajeteBadzNie() == false){
                if(temp.getObiektJedzony() != null){
                    temp.setJestNaNimPilkaBadzNie(true);
                    cachExec.execute(plansza.getPole(X, Y));
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    temp.getObiektJedzony().setZjedzonyBadzNie(true);
                    Y = Y - 1;
                    wynik = wynik + 1;
                }
                else {
                    plansza.getPole(X, Y).setZajeteBadzNie(false);
                    temp.setJestNaNimPilkaBadzNie(true);
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    cachExec.execute(plansza.getPole(X, Y));
                    Y = Y - 1;
                }
            }
            else {
                System.out.println("Pole jest zablokowane!");
            }
        }
        catch (NullPointerException e){
            System.out.println("Koniec planszy!");
        }
    }

    public void naLewo(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getLewy();
            if(temp.isZajeteBadzNie() == false){
                if(temp.getObiektJedzony() != null){
                    temp.setJestNaNimPilkaBadzNie(true);
                    cachExec.execute(plansza.getPole(X, Y));
                    temp.getObiektJedzony().setZjedzonyBadzNie(true);
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    X = X - 1;
                    wynik = wynik + 1;
                }
                else {
                    plansza.getPole(X, Y).setZajeteBadzNie(false);
                    temp.setJestNaNimPilkaBadzNie(true);
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    cachExec.execute(plansza.getPole(X, Y));
                    X = X - 1;
                }
            }
            else {
                System.out.println("Pole jest zablokowane, badz koniec planszy!");
            }
        }
        catch (NullPointerException e){
            System.out.println("Pole jest zablokowane, badz koniec planszy!");
        }
    }

    public void naPrawo(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getPrawy();
            if(temp.isZajeteBadzNie() == false){
                if(temp.getObiektJedzony() != null){
                    temp.setJestNaNimPilkaBadzNie(true);
                    cachExec.execute(plansza.getPole(X, Y));
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    temp.getObiektJedzony().setZjedzonyBadzNie(true);
                    X = X + 1;
                    wynik = wynik + 1;
                }
                else {
                    plansza.getPole(X, Y).setZajeteBadzNie(false);
                    temp.setJestNaNimPilkaBadzNie(true);
                    plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
                    cachExec.execute(plansza.getPole(X, Y));
                    X = X + 1;
                }
            }
            else {
                System.out.println("Pole jest zablokowane, badz koniec planszy!");
            }
        }
        catch (NullPointerException e){
            System.out.println("Pole jest zablokowane, badz koniec planszy!");
        }
    }
}
