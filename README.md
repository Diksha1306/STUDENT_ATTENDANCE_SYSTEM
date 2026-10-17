# STUDENT_ATTENDANCE_SYSTEM
The Student Attendance System is an application for maintaining student records, recording daily attendance, calculating attendance percentages, and generating reports. It replaces or supplements manual registers with organized digital records that can be searched and reviewed more easily.
from reportlab.lib.pagesizes import A4
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.enums import TA_CENTER
from reportlab.lib.units import cm
import os

out = "/mnt/data/Student_Attendance_System_Project.pdf"

doc = SimpleDocTemplate(
    out, pagesize=A4,
    rightMargin=1.7*cm, leftMargin=1.7*cm,
    topMargin=1.6*cm, bottomMargin=1.6*cm
)

navy = colors.HexColor("#17324D")
blue = colors.HexColor("#2F6B9A")
light_blue = colors.HexColor("#EAF2F8")
light_gray = colors.HexColor("#F4F6F8")
dark = colors.HexColor("#263238")
green_bg = colors.HexColor("#E5F4EC")
orange_bg = colors.HexColor("#FFF2DF")

styles = getSampleStyleSheet()
styles.add(ParagraphStyle(name="CoverTitle2", parent=styles["Title"], fontName="Helvetica-Bold",
                          fontSize=25, leading=31, textColor=navy, alignment=TA_CENTER, spaceAfter=12))
styles.add(ParagraphStyle(name="CoverSub2", parent=styles["Normal"], fontSize=12, leading=18,
                          textColor=blue, alignment=TA_CENTER, spaceAfter=12))
styles.add(ParagraphStyle(name="Section2", parent=styles["Heading1"], fontSize=16, leading=21,
                          textColor=navy, spaceBefore=7, spaceAfter=8))
styles.add(ParagraphStyle(name="SubSection2", parent=styles["Heading2"], fontSize=11.5, leading=15,
                          textColor=blue, spaceBefore=6, spaceAfter=4))
styles.add(ParagraphStyle(name="Body2", parent=styles["BodyText"], fontSize=9.2, leading=13.2,
                          textColor=dark, spaceAfter=5))
styles.add(ParagraphStyle(name="Small2", parent=styles["BodyText"], fontSize=7.8, leading=10.2,
                          textColor=dark, spaceAfter=3))
styles.add(ParagraphStyle(name="Code2", parent=styles["Code"], fontName="Courier",
                          fontSize=7.7, leading=10.5, backColor=light_gray, borderPadding=7,
                          leftIndent=7, rightIndent=7, spaceBefore=3, spaceAfter=7))
styles.add(ParagraphStyle(name="Callout2", parent=styles["BodyText"], fontSize=8.8, leading=12.5,
                          textColor=navy, backColor=light_blue, borderPadding=8, spaceBefore=4, spaceAfter=7))
P = lambda text, style="Body2": Paragraph(text, styles[style])

story = [
    Spacer(1, 2.2*cm),
    P("STUDENT ATTENDANCE SYSTEM", "CoverTitle2"),
    P("Project Documentation", "CoverSub2"),
    Spacer(1, 0.6*cm),
]

cover = Table([
    [P("<b>Project Type</b>", "Small2"), P("Student record and attendance management application", "Body2")],
    [P("<b>Primary Purpose</b>", "Small2"), P("Manage student records, daily attendance, attendance percentages, and reports", "Body2")],
    [P("<b>Suggested Language</b>", "Small2"), P("Python", "Body2")],
    [P("<b>Suggested Storage</b>", "Small2"), P("CSV or SQLite for a beginner-friendly version", "Body2")],
], colWidths=[4.0*cm, 10.8*cm])
cover.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,-1), light_blue),
    ("BOX", (0,0), (-1,-1), 0.8, colors.HexColor("#C7D8E6")),
    ("INNERGRID", (0,0), (-1,-1), 0.4, colors.white),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 9),
    ("RIGHTPADDING", (0,0), (-1,-1), 9),
    ("TOPPADDING", (0,0), (-1,-1), 8),
    ("BOTTOMPADDING", (0,0), (-1,-1), 5),
]))
story += [
    cover, Spacer(1, 0.9*cm),
    P("A beginner-friendly project report suitable for learning, documentation, and a GitHub portfolio.", "CoverSub2"),
    Spacer(1, 2.7*cm),
    P("<b>Implementation note:</b> This report describes a proposed design. Update the file names, database choice, and screenshots to match the version you build.", "Callout2"),
    PageBreak()
]

story += [
    P("1. Project Overview", "Section2"),
    P("The Student Attendance System is an application for maintaining student records, recording daily attendance, calculating attendance percentages, and generating reports. It replaces or supplements manual registers with organized digital records that can be searched and reviewed more easily.", "Body2"),
    P("A simple version can run in the terminal using Python and CSV files. A more robust version can use SQLite for structured storage and add a graphical or web interface for teachers and administrators.", "Body2"),
    P("2. Problem Statement", "Section2"),
    P("Manual attendance registers can be time-consuming to update and difficult to summarize. Calculating percentages for each student and preparing class-wise or date-wise reports can also lead to mistakes. A digital attendance system helps centralize records and makes summaries easier to produce.", "Body2"),
    P("3. Objectives", "Section2"),
]
for item in [
    "Store and maintain basic student details.",
    "Record daily attendance as Present or Absent.",
    "Prevent accidental duplicate entries for the same student, class, and date.",
    "Calculate attendance percentages automatically.",
    "Generate student-wise, class-wise, and date-wise reports.",
    "Make records easy to update while protecting student information."
]:
    story.append(P("• " + item, "Body2"))

story += [P("4. Main Features", "Section2")]
feature_rows = [
    [P("<b>Feature</b>", "Small2"), P("<b>Description</b>", "Small2")],
    [P("Student management", "Small2"), P("Add, view, edit, and search student ID, name, course, class, or section.", "Small2")],
    [P("Daily attendance", "Small2"), P("Mark each enrolled student Present or Absent for a selected date and class.", "Small2")],
    [P("Percentage calculation", "Small2"), P("Calculate attendance using sessions held and sessions attended.", "Small2")],
    [P("Reports", "Small2"), P("Display or export student-wise, date-wise, and class-wise summaries.", "Small2")],
    [P("Data validation", "Small2"), P("Check required fields, valid dates, and duplicate attendance records.", "Small2")],
]
ft = Table(feature_rows, colWidths=[4.0*cm, 10.8*cm], repeatRows=1)
ft.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy), ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 7), ("RIGHTPADDING", (0,0), (-1,-1), 7),
    ("TOPPADDING", (0,0), (-1,-1), 6), ("BOTTOMPADDING", (0,0), (-1,-1), 6),
]))
story += [ft, PageBreak()]

story += [
    P("5. System Architecture", "Section2"),
    P("The application receives student and attendance inputs, validates them, stores the records, calculates attendance summaries, and displays or exports reports.", "Body2")
]
arch_rows = [
    [P("<b>Teacher / Administrator</b>", "Body2")],
    [P("Adds student records and selects class and date", "Small2")],
    [P("↓", "CoverSub2")],
    [P("<b>Attendance Interface</b>", "Body2")],
    [P("Terminal menu or optional web interface", "Small2")],
    [P("↓", "CoverSub2")],
    [P("<b>Application Logic (Python)</b>", "Body2")],
    [P("Validate input, prevent duplicates, mark attendance, calculate percentages", "Small2")],
    [P("↓", "CoverSub2")],
    [P("<b>Data Storage</b>", "Body2")],
    [P("CSV files or SQLite database: students and attendance records", "Small2")],
    [P("↓", "CoverSub2")],
    [P("<b>Reports Module</b>", "Body2")],
    [P("Student summary, class summary, date-wise attendance, CSV export", "Small2")],
]
arch = Table(arch_rows, colWidths=[14.8*cm])
arch.setStyle(TableStyle([
    ("BOX", (0,0), (-1,-1), 0.7, colors.HexColor("#C7D8E6")),
    ("INNERGRID", (0,0), (-1,-1), 0.35, colors.HexColor("#E1E8EE")),
    ("BACKGROUND", (0,0), (-1,1), light_blue),
    ("BACKGROUND", (0,3), (-1,4), light_gray),
    ("BACKGROUND", (0,6), (-1,7), light_blue),
    ("BACKGROUND", (0,9), (-1,10), light_gray),
    ("BACKGROUND", (0,12), (-1,13), green_bg),
    ("ALIGN", (0,0), (-1,-1), "CENTER"),
    ("VALIGN", (0,0), (-1,-1), "MIDDLE"),
    ("LEFTPADDING", (0,0), (-1,-1), 8), ("RIGHTPADDING", (0,0), (-1,-1), 8),
    ("TOPPADDING", (0,0), (-1,-1), 4), ("BOTTOMPADDING", (0,0), (-1,-1), 4),
]))
story += [arch, Spacer(1, 8), P("6. Suggested Technology Stack", "Section2")]
tech_rows = [
    [P("<b>Component</b>", "Small2"), P("<b>Suggested option</b>", "Small2"), P("<b>Purpose</b>", "Small2")],
    [P("Programming", "Small2"), P("Python 3", "Small2"), P("Core application logic", "Small2")],
    [P("Storage", "Small2"), P("CSV or SQLite", "Small2"), P("Store student and attendance records", "Small2")],
    [P("Reports", "Small2"), P("Python csv module; optional pandas", "Small2"), P("Summaries and export", "Small2")],
    [P("Interface", "Small2"), P("Terminal first; Tkinter/Flask later", "Small2"), P("Data entry and viewing reports", "Small2")],
]
tech = Table(tech_rows, colWidths=[3.2*cm, 5.4*cm, 6.2*cm], repeatRows=1)
tech.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy), ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("VALIGN", (0,0), (-1,-1), "TOP"),
    ("LEFTPADDING", (0,0), (-1,-1), 6), ("RIGHTPADDING", (0,0), (-1,-1), 6),
    ("TOPPADDING", (0,0), (-1,-1), 5), ("BOTTOMPADDING", (0,0), (-1,-1), 5),
]))
story += [tech, PageBreak()]

story += [
    P("7. Data Design", "Section2"),
    P("A simple design can use two related tables or CSV files. The student record identifies each student; the attendance record links the student to a date and status.", "Body2"),
    P("Students table / students.csv", "SubSection2"),
    P("student_id, name, course, class_section", "Code2"),
    P("Attendance table / attendance.csv", "SubSection2"),
    P("student_id, date, status, subject_or_period", "Code2"),
    P("Use a unique attendance key such as student_id + date + subject_or_period to avoid recording the same attendance entry twice. In a real deployment, define how cancelled classes, leave, and excused absences are handled.", "Callout2"),
    P("8. Attendance Percentage Formula", "Section2"),
    P("Attendance percentage is usually calculated as:", "Body2"),
    P("<b>Attendance % = (Sessions Attended ÷ Total Sessions Held) × 100</b>", "Callout2"),
    P("Example: If a student attended 36 out of 40 classes, their attendance is (36 ÷ 40) × 100 = 90%. If no classes have been held, show “N/A” rather than dividing by zero.", "Body2"),
    P("9. Example Report", "Section2"),
]
report_rows = [
    [P("<b>Student ID</b>", "Small2"), P("<b>Name</b>", "Small2"), P("<b>Classes held</b>", "Small2"), P("<b>Attended</b>", "Small2"), P("<b>Attendance</b>", "Small2")],
    [P("ST101", "Small2"), P("Sample Student A", "Small2"), P("40", "Small2"), P("36", "Small2"), P("90%", "Small2")],
    [P("ST102", "Small2"), P("Sample Student B", "Small2"), P("40", "Small2"), P("30", "Small2"), P("75%", "Small2")],
    [P("ST103", "Small2"), P("Sample Student C", "Small2"), P("40", "Small2"), P("38", "Small2"), P("95%", "Small2")],
]
rt = Table(report_rows, colWidths=[2.5*cm, 4.2*cm, 2.7*cm, 2.4*cm, 3.0*cm], repeatRows=1)
rt.setStyle(TableStyle([
    ("BACKGROUND", (0,0), (-1,0), navy), ("TEXTCOLOR", (0,0), (-1,0), colors.white),
    ("ROWBACKGROUNDS", (0,1), (-1,-1), [colors.white, light_gray]),
    ("GRID", (0,0), (-1,-1), 0.4, colors.HexColor("#D5DDE3")),
    ("ALIGN", (2,1), (-1,-1), "CENTER"), ("VALIGN", (0,0), (-1,-1), "MIDDLE"),
    ("LEFTPADDING", (0,0), (-1,-1), 5), ("RIGHTPADDING", (0,0), (-1,-1), 5),
    ("TOPPADDING", (0,0), (-1,-1), 6), ("BOTTOMPADDING", (0,0), (-1,-1), 6),
]))
story += [rt, Spacer(1, 7), P("The values above are illustrative examples, not real student records.", "Small2"), PageBreak()]

story += [
    P("10. Implementation Plan", "Section2"),
]
for i, s in enumerate([
    "Create the project folder and a Python entry script such as main.py.",
    "Design student fields and create the student data file or SQLite table.",
    "Build menu options to add, list, search, and update student records.",
    "Add attendance marking by date and class or subject.",
    "Validate student IDs, dates, attendance statuses, and duplicate entries.",
    "Calculate attendance percentages from valid records.",
    "Generate student-wise and class-wise reports and allow CSV export.",
    "Test normal use, invalid input, missing records, and duplicate attendance."
], 1):
    story.append(P(f"{i}. {s}", "Body2"))

story += [
    P("11. Privacy and Limitations", "Section2"),
    P("• Attendance records are personal student information; restrict access to authorized users.<br/>• Do not upload real student names, IDs, or attendance data to a public GitHub repository.<br/>• Add authentication and backups before using the system in a real college.<br/>• Define rules for holidays, cancelled sessions, late arrivals, and approved leave.<br/>• Attendance percentages are only as accurate as the underlying records.", "Body2"),
    P("12. Testing Checklist", "Section2"),
]
for item in [
    "Can add and search a student using their ID",
    "Can mark attendance for a valid date",
    "Rejects or warns about duplicate entries",
    "Calculates percentages correctly",
    "Handles students with zero classes held",
    "Reports match the stored records",
    "Does not expose real student data in the public repository"
]:
    story.append(P("☐ " + item, "Body2"))

story += [
    P("13. Conclusion", "Section2"),
    P("The Student Attendance System is a useful beginner software project that combines record management, data validation, basic calculations, and report generation. Starting with Python and CSV or SQLite keeps it manageable, while the modular design makes it possible to add a graphical interface, authentication, and more detailed reports later.", "Body2"),
    P("Suggested GitHub repository structure", "SubSection2"),
    P("README.md • main.py • students.csv (sample data only) • attendance.csv (sample data only) • requirements.txt • architecture.png • project_report.pdf", "Callout2"),
]

def footer(canvas, doc):
    canvas.saveState()
    width, height = A4
    canvas.setStrokeColor(colors.HexColor("#D5DDE3"))
    canvas.line(1.7*cm, 1.15*cm, width-1.7*cm, 1.15*cm)
    canvas.setFont("Helvetica", 8)
    canvas.setFillColor(colors.HexColor("#667782"))
    canvas.drawString(1.7*cm, 0.75*cm, "Student Attendance System — Project Documentation")
    canvas.drawRightString(width-1.7*cm, 0.75*cm, f"Page {doc.page}")
    canvas.restoreState()

doc.build(story, onFirstPage=footer, onLaterPages=footer)
print(f"Created: {out}")
