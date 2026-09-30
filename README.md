
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[french]{babel}
\usepackage{amsmath,amssymb}
\usepackage{booktabs}
\usepackage{geometry}
\usepackage{float}
\usepackage{xcolor}
\usepackage{hyperref}
\usepackage{siunitx}

\geometry{margin=2.5cm}

\title{
    \textbf{Amélioration de la méthode INERIS (FNAP)}\\
    \large Modélisation du feu de nappe — Corrections et extensions
}
\author{
    \textbf{Nassima HANED}\\
    \small Ingénieur en Planification et Statistique
}
\date{\today}

\begin{document}
\maketitle

\begin{abstract}
Ce document présente une version améliorée de la méthode INERIS (logiciel FNAP, 1994) pour la modélisation du feu de nappe. La méthode originale repose sur cinq étapes : géométrie de la nappe, vitesse de combustion, hauteur de flamme (Thomas), puissance émissive (Mudan-Croce) et flux reçu. Dix corrections additives sont proposées pour pallier ses limites : intégration de la corrélation de Heskestad, facteur d'écran, terme convectif, méthode des sources multiples, Zabetakis-Burgess, effet du vent, calibration et approche Monte-Carlo. La version améliorée reste compatible avec la réglementation et les seuils (3, 5, 8~kW/m\textsuperscript{2}).
\end{abstract}

\tableofcontents
\newpage

% ============================================================
\section{Introduction}
% ============================================================

La méthode en cinq étapes utilisée pour la modélisation du feu de nappe est \textbf{identique à celle de l'INERIS} (logiciel FNAP, 1994). Elle repose sur les corrélations classiques de Zabetakis-Burgess, Thomas et Mudan-Croce. L'objectif de ce document est de proposer une \textbf{version améliorée} de cette méthode, en conservant sa structure et sa compatibilité réglementaire.

% ============================================================
\section{Rappel de la méthode originale}
% ============================================================

\subsection{Étape 1 — Géométrie de la nappe}

\[
D = \sqrt{\frac{4A}{\pi}}
\]

\subsection{Étape 2 — Vitesse de combustion}

\[
\dot{m}'' = 0{,}055 \text{ kg/m}^2\text{·s} \quad \text{(essence)}
\]

\subsection{Étape 3 — Hauteur de flamme (Thomas)}

\[
\frac{H}{D} = 42 \left[ \frac{\dot{m}''}{\rho_{\text{air}} \sqrt{g D}} \right]^{0{,}61}
\]

\subsection{Étape 4 — Puissance émissive (Mudan-Croce)}

\[
\text{SEP} = \text{SEP}_{\max} e^{-sD} + \text{SEP}_{\text{suie}} \left( 1 - e^{-sD} \right)
\]

Avec $\text{SEP}_{\max} = 140$~kW/m\textsuperscript{2}, $\text{SEP}_{\text{suie}} = 20$~kW/m\textsuperscript{2}, $s = 0{,}12$~m\textsuperscript{-1}.

\subsection{Étape 5 — Flux reçu}

\[
q = \text{SEP} \cdot \tau \cdot F
\]

Avec $\tau = 2{,}02 (P_w d)^{-0{,}09}$ et $F$ le facteur de vue.

% ============================================================
\section{Limites de la méthode originale}
% ============================================================

\begin{table}[H]
\centering
\caption{Limites identifiées de la méthode INERIS (FNAP)}
\begin{tabular}{@{}cl@{}}
\toprule
N° & Limite \\
\midrule
1 & Flamme cylindrique idéalisée \\
2 & Absence d'obstacles \\
3 & Calcul purement radiatif \\
4 & Géométries de nappe limitées \\
5 & Domaine de validité restreint \\
6 & Sensibilité à la surface \\
7 & Vitesse de combustion constante \\
8 & Conditions météorologiques figées \\
9 & Écarts avec l'expérimental (6--23\%) \\
10 & Fiabilité statistique faible \\
\bottomrule
\end{tabular}
\end{table}

% ============================================================
\section{Corrections proposées}
% ============================================================

\subsection{Correction 1 — Corrélation de Heskestad}

Pour les grandes nappes ($D > 20$~m), remplacer Thomas par Heskestad :

\[
L = 0{,}235 \dot{Q}^{2/5} - 1{,}02 D
\]

Avec $\dot{Q} = \dot{m}'' \cdot A \cdot \Delta H_c$ la puissance totale (kW).

\subsection{Correction 2 — Facteur d'écran}

Pour tenir compte des obstacles (murs, auvents, bâtiments) :

\[
q_{\text{corrigé}} = q \times \eta_{\text{écran}}
\]

\begin{table}[H]
\centering
\caption{Facteurs d'écran}
\begin{tabular}{@{}lc@{}}
\toprule
Configuration & $\eta_{\text{écran}}$ \\
\midrule
Pas d'obstacle & 1,0 \\
Auvent partiel & 0,7 \\
Mur plein & 0,3 \\
Écran total & 0,1 \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Correction 3 — Terme convectif}

En champ proche ($< 5$~m), ajouter la convection :

\[
q_{\text{total}} = q_{\text{rad}} + q_{\text{conv}}
\]

Avec $q_{\text{conv}} = h (T_f - T_c)$, $h = 10$ à $50$~W/m\textsuperscript{2}·K.

\subsection{Correction 4 — Sources multiples}

Pour les géométries complexes, diviser la nappe en $N$ sous-nappes circulaires :

\[
q_{\text{total}}(x) = \sum_{i=1}^{N} q_i(x)
\]

\subsection{Correction 5 — Extension du domaine de validité}

\begin{table}[H]
\centering
\caption{Choix du modèle selon le diamètre}
\begin{tabular}{@{}lc@{}}
\toprule
Diamètre & Modèle recommandé \\
\midrule
$D < 20$~m & Thomas \\
$20 < D < 50$~m & Thomas ou Heskestad \\
$D > 50$~m & Heskestad ou CFD \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Correction 6 — Analyse de sensibilité}

Calculer les distances pour trois scénarios de surface :

\begin{table}[H]
\centering
\caption{Scénarios de surface}
\begin{tabular}{@{}lc@{}}
\toprule
Scénario & Surface \\
\midrule
Optimiste & $0{,}5 A$ \\
Nominal & $A$ \\
Pessimiste & $1{,}5 A$ \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Correction 7 — Zabetakis-Burgess}

Remplacer la vitesse de combustion constante par :

\[
\dot{m}''(D) = \dot{m}''_{\infty} \left( 1 - e^{-k\beta D} \right)
\]

Pour l'essence : $\dot{m}''_{\infty} = 0{,}055$~kg/m\textsuperscript{2}·s, $k\beta = 2{,}1$~m\textsuperscript{-1}.

\subsection{Correction 8 — Effet du vent}

Utiliser la version de Thomas avec vent :

\[
L = 19{,}18 \times \dot{m}^{0{,}74} \times D^{0{,}735}
\]

Et calculer l'inclinaison :

\[
\cos\theta = \begin{cases} 1 & \text{si } u^* \leq 1 \\ (u^*)^{-0{,}5} & \text{si } u^* > 1 \end{cases}
\]

Avec $u^* = u / (g \dot{m}'' D / \rho_v)^{1/3}$.

\subsection{Correction 9 — Facteur de calibration}

\[
q_{\text{calibré}} = C_{\text{cal}} \times q_{\text{calculé}}
\]

\begin{table}[H]
\centering
\caption{Facteurs de calibration}
\begin{tabular}{@{}lc@{}}
\toprule
Source & $C_{\text{cal}}$ \\
\midrule
INERIS & 1,0 \\
Essais PROSERPINE & 0,9 \\
RETEX & 0,85 \\
\bottomrule
\end{tabular}
\end{table}

\subsection{Correction 10 — Approche Monte-Carlo}

Remplacer la valeur unique par une distribution :

\[
\dot{m}'' \sim \mathcal{N}(0{,}055, 0{,}01)
\]

Puis simuler Monte-Carlo pour obtenir une distribution des distances.

% ============================================================
\section{Formule améliorée}
% ============================================================

\[
\boxed{
q_{\text{amélioré}} = \eta_{\text{écran}} \times C_{\text{cal}} \times \left( q_{\text{rad}} + q_{\text{conv}} \right)
}
\]

Avec :
\begin{align*}
q_{\text{rad}} &= \text{SEP} \times \tau \times F \\
q_{\text{conv}} &= h (T_f - T_c)
\end{align*}

% ============================================================
\section{Comparaison INERIS vs INERIS amélioré}
% ============================================================

\begin{table}[H]
\centering
\caption{Comparaison des deux versions}
\begin{tabular}{@{}lcc@{}}
\toprule
Élément & INERIS (FNAP) & INERIS amélioré \\
\midrule
Géométrie & $D = \sqrt{4A/\pi}$ & Idem \\
$\dot{m}''$ & Constante & Zabetakis-Burgess \\
Hauteur de flamme & Thomas & Thomas ou Heskestad \\
Pouvoir émissif & Mudan-Croce & Idem \\
Transmissivité & Bagster & Idem \\
Facteur de vue & Mudan & Idem \\
Obstacles & Non & $\eta_{\text{écran}}$ \\
Convection & Non & $q_{\text{conv}}$ \\
Vent & Fixe 5~m/s & Variable \\
Incertitude & Non & Monte-Carlo \\
Calibration & Non & $C_{\text{cal}}$ \\
Seuils & 3, 5, 8~kW/m\textsuperscript{2} & Idem \\
\bottomrule
\end{tabular}
\end{table}

% ============================================================
\section{Ce que cela apporte}
% ============================================================

\begin{table}[H]
\centering
\caption{Apports de la version améliorée}
\begin{tabular}{@{}ll@{}}
\toprule
Apport & Détail \\
\midrule
Précision & Meilleure pour petites nappes \\
Réalisme & Obstacles et convection intégrés \\
Flexibilité & Géométries complexes \\
Incertitude & Quantifiée \\
Traçabilité & Corrections documentées \\
Compatibilité & Réglementation respectée \\
\bottomrule
\end{tabular}
\end{table}

% ============================================================
\section{Conclusion}
% ============================================================

La méthode en cinq étapes utilisée dans ce travail est \textbf{identique à celle de l'INERIS} (logiciel FNAP). Dans une démarche d'amélioration, dix corrections sont proposées : utilisation de Heskestad pour les grandes nappes, facteur d'écran pour les obstacles, terme convectif en champ proche, méthode des sources multiples, Zabetakis-Burgess pour la vitesse de combustion, version de Thomas avec vent, facteur de calibration, et approche Monte-Carlo.

Ces corrections sont \textbf{additives} : elles ne cassent pas la méthode originale, mais l'enrichissent. La version améliorée reste compatible avec la réglementation et les seuils (3, 5, 8~kW/m\textsuperscript{2}).

% ============================================================
\section*{Références}
% ============================================================

\begin{itemize}
    \item Zabetakis, M.G., Burgess, D.S. (1961). \textit{Research on the hazards associated with the production and handling of liquid hydrogen}. US Bureau of Mines.
    \item Thomas, P.H. (1963). \textit{The size of flames from natural fires}. 9th Symposium on Combustion.
    \item Mudan, K.S., Croce, P.A. (1980). \textit{Fire hazard calculations for large open hydrocarbon fires}. SFPE Handbook.
    \item INERIS (1994). \textit{Logiciel FNAP — Feux de nappes}. Rapport technique.
    \item Heskestad, G. (1983). \textit{Luminous heights of turbulent diffusion flames}. Fire Safety Journal.
\end{itemize}

\end{document}
