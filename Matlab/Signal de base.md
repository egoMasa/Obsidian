
# 1)Répresenter signal sur axe de temps

periode = -2*pi:.01:2*pi ou periode = linspace(-2*pi,2*pi)
signal = sin(periode)
plot(periode,signal)
grid on
title('Representation signal domaine temporel')
legend('Signal sinusoidale ');

# 2)Répresenter signal sur axe des fréquences:

A savoir
-On definit la fréquence du signal d'échantillonnage de
-On défnit le temps qui va de 0 à 1 par pas de 1/fe
-On définit la fréquence du signal Fo
-Générer signal carré : carre = square(2*pi*f*t), plot(t,carre)
-Générer signal dent de scie : dent = sawtooth(2*pi*f*t), plot(t,dent)
-Générer signal sinus : sinus = sin(2*pi*f*t), plot(t,sinus)
-Générer signal sinus qui accelère : sin2 = chirp(2*pi*f*t), plot(t,sin2)
-Générer escalier = stairs(signal)