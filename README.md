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

	interface ILogicStep {
		void runStep(PoleNaPlanszy pole, int X, int Y);
	}
	
	Map<String, ILogicStep> ballSteps = new HashMap<String, ILogicStep>();

    public Ball(Plansza planszaParam){
    
    		ballSteps.put("falsetrue", new ILogicStep() {
			@Override
			public void runStep(PoleNaPlanszy pole, int x, int y) {
				pole.setJestNaNimPilkaBadzNie(true);
				cachExec.execute(plansza.getPole(X, Y));
				plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
				pole.getObiektJedzony().setZjedzonyBadzNie(true);
				Y = y;
				X = x;
				wynik = wynik + 1;
			}
		});
		
		ballSteps.put("falsefalse", new ILogicStep() { 
			@Override
			public void runStep(PoleNaPlanszy pole, int x, int y) {
				plansza.getPole(X, Y).setZajeteBadzNie(false);
				pole.setJestNaNimPilkaBadzNie(true);
				plansza.getPole(X, Y).setJestNaNimPilkaBadzNie(false);
				cachExec.execute(plansza.getPole(X, Y));
				Y = y;
				X = x;
			}
		});

		ballSteps.put("truefalse", new ILogicStep() {
			@Override
			public void runStep(PoleNaPlanszy pole, int x, int y) {
			}
		});

		ballSteps.put("truetrue", new ILogicStep() {
			@Override
			public void runStep(PoleNaPlanszy pole, int x, int y) {
			}
		});

        this.plansza = planszaParam;
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

    public void wGore() {
		try {
			PoleNaPlanszy temp = plansza.getPole(X, Y).getGora();
			ballSteps.get(temp.isZajeteBadzNie() + "" + temp.getObiektJedzony() != null).runStep(temp, X, Y+1); 
		} catch (NullPointerException e) {
			System.out.println("Pole jest zablokowane, badz koniec planszy!");
		}
	}

	public void wDol() {
		try {
			PoleNaPlanszy temp = plansza.getPole(X, Y).getDol();
			ballSteps.get(temp.isZajeteBadzNie() + "" + temp.getObiektJedzony() != null).runStep(temp, X, Y-1); 
		} catch (NullPointerException e) {
			System.out.println("Koniec planszy!");
		}
	}
    
    public void naLewo(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getLewy();
			ballSteps.get(temp.isZajeteBadzNie() + "" + temp.getObiektJedzony() != null).runStep(temp, X-1, Y); 
        }
        catch (NullPointerException e){
            System.out.println("Pole jest zablokowane, badz koniec planszy!");
        }
    }

    public void naPrawo(){
        try {
            PoleNaPlanszy temp = plansza.getPole(X, Y).getPrawy();
			ballSteps.get(temp.isZajeteBadzNie() + "" + temp.getObiektJedzony() != null).runStep(temp, X+1, Y); 
        }
        catch (NullPointerException e){
            System.out.println("Pole jest zablokowane, badz koniec planszy!");
        }
    }
}
