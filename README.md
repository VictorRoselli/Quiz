# Quiz

//
//  ViewController.swift
//  FirstQuiz
//
//  Created by Victor Roselli on 11/2/15.
//  Copyright © 2015 Victor Roselli. All rights reserved.
//

import UIKit
import Foundation


class ViewController: UIViewController {

    

    @IBOutlet weak var lblQuestion: UILabel!
    @IBOutlet weak var imgQuestion: UIImageView!
    @IBOutlet weak var btnAnswer1: UIButton!
    @IBOutlet weak var btnAnswer2: UIButton!
    @IBOutlet weak var btnAnswer3: UIButton!
    @IBOutlet weak var btnResposta4: UIButton!
    
    @IBOutlet weak var viewfeedback: UIView!
    
    @IBOutlet weak var lblfeedback: UILabel!
    @IBOutlet weak var btnfeedback: UIButton!

    

    
    var questions : [Question]! // vetor que contém as questões do quiz
    @IBOutlet weak var imgFeedback: UIImageView!
    var currentQuestion = 0 // indica qual a questão atual INT
    var grade = 0.0 // cálculo da nota DOUBLE
    var quizEnded = false // inidica se o quiz terminou ou não BOOL

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        
        let q0answer0 = Answer(answer: "AWD", isCorrect: true)
        let q0answer1 = Answer(answer: "Traseira", isCorrect: false)
        let q0answer2 = Answer(answer: "Dianteira", isCorrect: false)
        let q0answer3 = Answer(answer: "NDA", isCorrect: false)
        let question0 = Question(question: "Qual o tipo de tração do Porsche Boxter GTS ?", strImageFileName: "thumbnail", answers: [q0answer0,q0answer1,q0answer2,q0answer3])
        
        let q1answer0 = Answer(answer: "4.1 Turbo", isCorrect: false)
        let q1answer1 = Answer(answer: "5.0 Turbo Diesel", isCorrect: false)
        let q1answer2 = Answer(answer: "3.0 V6 Biturbo", isCorrect: true)
        let q1answer3 = Answer(answer: "2.8 V8 Biturbo", isCorrect: false)
        let question1 = Question(question: "Qual a motorização da Maserati Ghibi ?", strImageFileName: "Maserati", answers: [q1answer0,q1answer1,q1answer2,q1answer3])
        
        let q2answer0 = Answer(answer: "550 cv", isCorrect: false)
        let q2answer1 = Answer(answer: "700 cv", isCorrect: true)
        let q2answer2 = Answer(answer: "350 cv", isCorrect: false)
        let q2answer3 = Answer(answer: "400 cv", isCorrect: false)
        let question2 = Question(question: "Qual a potência da Lamborguini Aventator ?", strImageFileName: "Lamborguini", answers: [q2answer0,q2answer1,q2answer2,q2answer3])
        
        let q3answer0 = Answer(answer: "9 marchas", isCorrect: false)
        let q3answer1 = Answer(answer: "7 marchas", isCorrect: true)
        let q3answer2 = Answer(answer: "6 marchas", isCorrect: false)
        let q3answer3 = Answer(answer: "8 marchas", isCorrect: false)
        let question3 = Question(question: "Quantas marchas tem uma Ferrari 458 Italia ?", strImageFileName: "Ferrari", answers: [q3answer0,q3answer1,q3answer2,q3answer3])
        
        let q4answer0 = Answer(answer: "Michael Schumacher", isCorrect: false)
        let q4answer1 = Answer(answer: "Ayrton Senna", isCorrect: true)
        let q4answer2 = Answer(answer: "Fernando Alonso", isCorrect: false)
        let q4answer3 = Answer(answer: "Alain Prost", isCorrect: false)
        let question4 = Question(question: "Qual Piloto da Fórmula 1 ajudou no desenvolvimento do Honda NSX ?", strImageFileName: "Honda_NSX", answers: [q4answer0,q4answer1,q4answer2,q4answer3])
        
        let q5answer0 = Answer(answer: "Tony Stark - Iron Man", isCorrect: false)
        let q5answer1 = Answer(answer: "James Bond - 007", isCorrect: true)
        let q5answer2 = Answer(answer: "Bruce Wayne - Batman", isCorrect: false)
        let q5answer3 = Answer(answer: "Klark Kent - Superman", isCorrect: false)
        let question5 = Question(question: "Qual famoso herói usa um Aston Martin Vanquish ?", strImageFileName: "Aston", answers: [q5answer0,q5answer1,q5answer2,q5answer3])
        
        
        questions = [question0,question1,question2,question3,question4,question5]
        startQuiz() // começa o quiz
        
    }
    
    func startQuiz(){
        imgQuestion.hidden = false
        questions.shuffle()
        for ( var i=0; i<questions.count;i++){
            questions[i].answers.shuffle()
        }
        
        // reseta as variáveis de progresso
        quizEnded = false
        grade = 0.0
        currentQuestion = 0
        
        showQuestion(0) // mostra primeira questão
    }
    
    func showQuestion (questionid : Int){
        
        btnAnswer1.enabled = true
        btnAnswer2.enabled = true
        btnAnswer3.enabled = true
        btnAnswer4.enabled = true
        
        lblQuestion.text = questions[questionid].strQuestion
        imgQuestion.image = questions[questionid].imgQuestion
        btnAnswer1.setTitle(questions[questionid].answers[0].strAnswer, forState: UIControlState.Normal)
        btnAnswer2.setTitle(questions[questionid].answers[1].strAnswer, forState: UIControlState.Normal)
        btnAnswer3.setTitle(questions[questionid].answers[2].strAnswer, forState: UIControlState.Normal)
        btnAnswer4.setTitle(questions[questionid].answers[3].strAnswer, forState: UIControlState.Normal)
        
    }
    
    func selectAnswer (answerid : Int){
        
        btnAnswer1.enabled = false
        btnAnswer2.enabled = false
        btnAnswer3.enabled = false
        
        viewfeedback.hidden = false
        
        let answer : Answer = questions[currentQuestion].answers[answerid] // seleciona resposta
        
        if (answer.isCorrect == true){
            grade = grade + 1
            imgFeedback.image = UIImage (named: "OK")
            viewfeedback.backgroundColor = UIColor.whiteColor()
            lblfeedback.text = "Resposta certa !"
        }else{
            imgFeedback.image = UIImage (named: "NOK")
            viewfeedback.backgroundColor = UIColor.whiteColor()
            lblfeedback.text = "Resposta errada !"
        }
        
        if(currentQuestion < questions.count-1){
            btnfeedback.setTitle("Próxima", forState:UIControlState.Normal)
        }else{
            btnfeedback.setTitle("Ver nota", forState:UIControlState.Normal)
        }
    }
    
    @IBAction func chooseA1(sender: AnyObject) {
        selectAnswer(0)
    }
    
    @IBAction func chooseA2(sender: AnyObject) {
        selectAnswer(1)
    }
    
    @IBAction func chooseA3(sender: AnyObject) {
        selectAnswer(2)
    }

    
    @IBAction func btnfeedbackAction(sender: AnyObject) {
        viewfeedback.hidden = true
        
        if quizEnded{
            startQuiz()
        }else{
            nextQuestion()
        }
    }


    func nextQuestion(){
        currentQuestion++
        
        if currentQuestion < questions.count{
            showQuestion(currentQuestion)
        }else{
            endQuiz()
        }
}

    func endQuiz(){

        grade = grade / Double(questions.count) * 100
        quizEnded = true
        viewfeedback.hidden = false
        viewfeedback.backgroundColor = UIColor.whiteColor()
        imgFeedback.image = UIImage (named: "Final")
        imgQuestion.hidden = true
        lblfeedback.textColor = UIColor.blackColor()
        lblfeedback.text = "Sua nota:\(grade) "
        btnfeedback.setTitle("Refazer", forState: UIControlState.Normal)

    }

}
