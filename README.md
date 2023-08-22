# CRMFF

![qweqqweqwe](https://github.com/adil-nauruzbaev/CRMFF/assets/106264625/307f01eb-701e-4265-a3f2-94d11c83e6f5)

Custom Calendar Widget 

![qweqweeee](https://github.com/adil-nauruzbaev/CRMFF/assets/106264625/e8dcdb74-7f02-45de-9587-1a37ccfb2ed6)


```dart
    // Automatic FlutterFlow imports
import '/backend/backend.dart';
import '/flutter_flow/flutter_flow_theme.dart';
import '/flutter_flow/flutter_flow_util.dart';
import '/custom_code/widgets/index.dart'; // Imports other custom widgets
import '/flutter_flow/custom_functions.dart'; // Imports custom functions
import 'package:flutter/material.dart';
// Begin custom widget code
// DO NOT REMOVE OR MODIFY THE CODE ABOVE!

import 'package:collection/collection.dart';

class NewCustomWidget2 extends StatefulWidget {
  const NewCustomWidget2({
    Key? key,
    this.width,
    this.height,
    required this.listOfstatusFilled,
    required this.listOfstatusNotWorked,
    required this.listOfstatusUnfilled,
    required this.listOFstatusVacation,
    required this.listOfdate,
  }) : super(key: key);

  final double? width;
  final double? height;
  final List<bool> listOfstatusFilled;
  final List<bool> listOfstatusNotWorked;
  final List<bool> listOfstatusUnfilled;
  final List<bool> listOFstatusVacation;
  final List<DateTime> listOfdate;

  @override
  _NewCustomWidget2State createState() => _NewCustomWidget2State();
}

class _NewCustomWidget2State extends State<NewCustomWidget2> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // TRY THIS: Try running your application with "flutter run". You'll see
        // the application has a blue toolbar. Then, without quitting the app,
        // try changing the seedColor in the colorScheme below to Colors.green
        // and then invoke "hot reload" (save your changes or press the "hot
        // reload" button in a Flutter-supported IDE, or press "r" if you used
        // the command line to start the app).
        //
        // Notice that the counter didn't reset back to zero; the application
        // state is not lost during the reload. To reset the state, use hot
        // restart instead.
        //
        // This works for code too, not just values: Most code changes can be
        // tested with just a hot reload.
        colorScheme: ColorScheme.fromSeed(
            seedColor: Colors.deepPurple,
            primary: Colors.transparent,
            background: Colors.transparent),
        useMaterial3: true,
      ),
      home: MyCalendarCRM(
        data: List.generate(
          widget.listOfdate.length,
          (index) => DataStatus(
            date: widget.listOfdate[index],
            statusFilled: widget.listOfstatusFilled[index],
            statusNotWorked: widget.listOfstatusNotWorked[index],
            statusUnfilled: widget.listOfstatusUnfilled[index],
            statusVacation: widget.listOFstatusVacation[index],
          ),
        ),
        onTapDay: (date) {
          print(date);
        },
      ),
    );
  }
}

class MyCalendarCRM extends StatefulWidget {
  const MyCalendarCRM({
    super.key,
    required this.data,
    required this.onTapDay,
  });

  final Function(DateTime) onTapDay;
  final List<DataStatus> data;

  @override
  State<MyCalendarCRM> createState() => _MyCalendarCRMState();
}

class _MyCalendarCRMState extends State<MyCalendarCRM> {
  DateTime _selectedDay = DateTime.now();
  List<CalendarValue> _calendar = <CalendarValue>[];

  @override
  initState() {
    super.initState();
    _calendar = _initCalendar;
  }

  /// init calendar
  List<CalendarValue> get _initCalendar {
    List<CalendarValue> calendar = <CalendarValue>[];

    /// generate calendar
    if (!_isMonday(1)) {
      for (int i =
              _lastDayPreviousMonth.day + 1 - _untilFirstDayOfCurrentMonth(1);
          i < _lastDayPreviousMonth.day + 1;
          i++) {
        final dataStatus = _dataStatus(_lastDayPreviousMonth, i);
        calendar.add(CalendarValue(
          day: i,
          month: _lastDayPreviousMonth.month,
          year: _lastDayPreviousMonth.year,
          isPreviousMonthDay: true,
          isWeekend: _isHoliday(i, currentMonth: false),
          statusFilled: dataStatus?.statusFilled ?? false,
          statusNotWorked: dataStatus?.statusNotWorked ?? false,
          statusUnfilled: dataStatus?.statusUnfilled ?? false,
          statusVacation: dataStatus?.statusVacation ?? false,
        ));
      }
    }
    for (int i = 1; i < _lastDayCurrentMonth.day + 1; i++) {
      final dataStatus = _dataStatus(_lastDayCurrentMonth, i);
      calendar.add(CalendarValue(
        day: i,
        month: _lastDayCurrentMonth.month,
        year: _lastDayCurrentMonth.year,
        isWeekend: _isHoliday(i),
        statusFilled: dataStatus?.statusFilled ?? false,
        statusNotWorked: dataStatus?.statusNotWorked ?? false,
        statusUnfilled: dataStatus?.statusUnfilled ?? false,
        statusVacation: dataStatus?.statusVacation ?? false,
      ));
    }

    if (!_isSunday(_lastDayCurrentMonth.day)) {
      for (int i = 1;
          i < 7 - _untilFirstDayOfCurrentMonth(_lastDayCurrentMonth.day);
          i++) {
        final dataStatus = _dataStatus(_nextMonth, i);
        calendar.add(CalendarValue(
          day: i,
          month: _nextMonth.month,
          year: _nextMonth.year,
          isWeekend: i == _lastDayCurrentMonth.day,
          isNextMonthDay: true,
          statusFilled: dataStatus?.statusFilled ?? false,
          statusNotWorked: dataStatus?.statusNotWorked ?? false,
          statusUnfilled: dataStatus?.statusUnfilled ?? false,
          statusVacation: dataStatus?.statusVacation ?? false,
        ));
      }
    }
    return calendar;
  }

  DataStatus? _dataStatus(DateTime date, int day) =>
      widget.data.firstWhereOrNull(
        (e) =>
            DateTime(e.date.year, e.date.month, e.date.day) ==
            DateTime(date.year, date.month, day),
      );

  int _untilFirstDayOfCurrentMonth(int day) =>
      ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'].indexOf(_weeks(day));

  bool _isHoliday(int day, {bool currentMonth = true}) =>
      ['Sat', 'Sun'].contains(_weeks(day, currentMonth: currentMonth));

  bool _isMonday(int day) => ['Mon'].contains(_weeks(day));

  bool _isSunday(int day) => ['Sun'].contains(_weeks(day));

  String _weeks(int day, {bool currentMonth = true}) =>
      DateFormat.E().format(DateTime(
        (currentMonth ? _selectedDay : _previousMonth).year,
        (currentMonth ? _selectedDay : _previousMonth).month,
        day,
      ));

  /// last day of current month
  DateTime get _lastDayCurrentMonth => (_selectedDay.month < 12)
      ? DateTime(_selectedDay.year, _selectedDay.month + 1, 0)
      : DateTime(_selectedDay.year + 1, 1, 0);

  /// last day of previous month
  DateTime get _lastDayPreviousMonth =>
      DateTime(_selectedDay.year, _selectedDay.month, 0);

  DateTime get _previousMonth => (_selectedDay.month == 1)
      ? DateTime(_selectedDay.year - 1, 12)
      : DateTime(_selectedDay.year, _selectedDay.month - 1);

  DateTime get _nextMonth => (_lastDayCurrentMonth.month + 1 < 12)
      ? DateTime(_lastDayCurrentMonth.year, _lastDayCurrentMonth.month + 1)
      : DateTime(_lastDayCurrentMonth.year + 1, 1);

  Color _colors({
    required bool isNextMonthDay,
    required bool isPreviousMonthDay,
    required bool isWeekend,
    required bool isSelected,
  }) {
    if ((isNextMonthDay || isPreviousMonthDay || isWeekend) && !isSelected) {
      return const Color(0xFF727F88);
    }

    if (isSelected) return const Color(0xFFFFFFFF);

    return const Color(0xFF323F59);
  }

  Color _colorsSelected({
    required bool isSelected,
  }) {
    if (isSelected) return const Color(0xFFFF7348);
    return const Color(0xFFFFFFFF);
  }

  Color _colorsStatus({
    required bool statusFilled,
    required bool statusUnfilled,
    required bool statusVacation,
    required bool statusNotWorked,
  }) {
    if (statusFilled) return const Color(0xFF00C77F);
    if (statusUnfilled) return const Color(0xFFEE2222);
    if (statusVacation) return const Color(0xFF4849F1);
    if (statusNotWorked) return Colors.orange;

    return const Color(0xFFFFFFFF);
  }

  void movePreviousMonth() => setState(
        () {
          _selectedDay = (_selectedDay.month != 1)
              ? DateTime(
                  _selectedDay.year,
                  _selectedDay.month - 1,
                  _selectedDay.day,
                )
              : DateTime(_selectedDay.year - 1, 12, _selectedDay.day);
          _calendar = _initCalendar;
          FFAppState().update(() {
            FFAppState().focusedDayAppState = _selectedDay;
          });
        },
      );

  void moveNextMonth() => setState(
        () {
          _selectedDay = (_selectedDay.month < 12)
              ? DateTime(
                  _selectedDay.year,
                  _selectedDay.month + 1,
                  _selectedDay.day,
                )
              : DateTime(_selectedDay.year + 1, 1, _selectedDay.day);
          _calendar = _initCalendar;
          FFAppState().update(() {
            FFAppState().focusedDayAppState = _selectedDay;
          });
        },
      );

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Padding(
        padding: const EdgeInsets.all(0.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                MainCalendarButton(
                  onPressed: movePreviousMonth,
                  icon: Icons.chevron_left,
                ),
                Column(
                  children: [
                    Text(
                      DateFormat.MMMM().format(_selectedDay),
                      textAlign: TextAlign.center,
                      style: const TextStyle(
                        fontSize: 20,
                        color: Color(0xFF323F59),
                        fontWeight: FontWeight.w500,
                      ),
                    ),
                    Text(
                      _selectedDay.year.toString(),
                      textAlign: TextAlign.center,
                      style: const TextStyle(
                        fontSize: 12,
                        color: Color(0xFF727F88),
                        fontWeight: FontWeight.w400,
                      ),
                    ),
                  ],
                ),
                MainCalendarButton(
                  onPressed: moveNextMonth,
                  icon: Icons.chevron_right,
                ),
              ],
            ),
            const SizedBox(height: 28.0),
            Row(
              children: ['П', 'В', 'С', 'Ч', 'П', 'С', 'В']
                  .map(
                    (e) => SizedBox(
                      height: 28,
                      width: (MediaQuery.of(context).size.width - 32) / 7,
                      child: Text(
                        e,
                        textAlign: TextAlign.center,
                        style: const TextStyle(
                          fontWeight: FontWeight.w600,
                          fontSize: 14,
                          color: Color(0xFF727F88),
                        ),
                      ),
                    ),
                  )
                  .toList(),
            ),
            const SizedBox(height: 24.0),
            Wrap(
              children: [
                ...List.generate(
                  _calendar.length,
                  (index) {
                    final data = _calendar[index];
                    final isSelected = DateTime(
                          _selectedDay.year,
                          _selectedDay.month,
                          _selectedDay.day,
                        ) ==
                        DateTime(data.year, data.month, data.day);
                    return SizedBox(
                      width: (MediaQuery.of(context).size.width - 32) / 7,
                      child: Column(
                        children: [
                          InkWell(
                            onTap: () {
                              final date = DateTime(
                                data.year,
                                data.month,
                                data.day,
                              );
                              if (date.isBefore(DateTime.now())) {
                                setState(() => _selectedDay = date);
                              }
                              widget.onTapDay.call(_selectedDay);
                              FFAppState().update(() {
                                FFAppState().focusedDayAppState = _selectedDay;
                              });
                            },
                            borderRadius: BorderRadius.circular(6),
                            child: Ink(
                              width: 28,
                              height: 28,
                              decoration: BoxDecoration(
                                borderRadius: BorderRadius.circular(6),
                                color: _colorsSelected(
                                  isSelected: isSelected,
                                ),
                                border: Border.all(
                                  color: _colorsSelected(
                                    isSelected: isSelected,
                                  ),
                                ),
                              ),
                              child: Center(
                                child: Text(
                                  data.day.toString(),
                                  textAlign: TextAlign.center,
                                  style: TextStyle(
                                    fontSize: 14,
                                    fontWeight: FontWeight.w400,
                                    color: _colors(
                                      isWeekend: data.isWeekend,
                                      isNextMonthDay: data.isNextMonthDay,
                                      isPreviousMonthDay:
                                          data.isPreviousMonthDay,
                                      isSelected: isSelected,
                                    ),
                                  ),
                                ),
                              ),
                            ),
                          ),
                          const SizedBox(height: 8),
                          CircleAvatar(
                            backgroundColor: _colorsStatus(
                              statusFilled: data.statusFilled,
                              statusVacation: data.statusVacation,
                              statusUnfilled: data.statusUnfilled,
                              statusNotWorked: data.statusNotWorked,
                            ),
                            radius: 3,
                          ),
                        ],
                      ),
                    );
                  },
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

class MainCalendarButton extends StatelessWidget {
  const MainCalendarButton({
    super.key,
    required this.icon,
    required this.onPressed,
  });

  final IconData icon;
  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return InkWell(
      onTap: onPressed,
      borderRadius: BorderRadius.circular(6),
      child: Ink(
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(6),
          border: Border.all(
            color: const Color(0xFFE9EEF1),
          ),
        ),
        child: Icon(icon),
      ),
    );
  }
}

class CalendarValue {
  final int day;
  final int month;
  final int year;
  final bool isWeekend;
  final bool isPreviousMonthDay;
  final bool isNextMonthDay;
  final bool statusFilled;
  final bool statusUnfilled;
  final bool statusVacation;
  final bool statusNotWorked;

  CalendarValue({
    required this.day,
    required this.month,
    required this.year,
    required this.isWeekend,
    this.isPreviousMonthDay = false,
    this.isNextMonthDay = false,
    this.statusFilled = false,
    this.statusUnfilled = false,
    this.statusVacation = false,
    this.statusNotWorked = false,
  });
}

class DataStatus {
  final DateTime date;
  final bool statusFilled;
  final bool statusUnfilled;
  final bool statusVacation;
  final bool statusNotWorked;

  DataStatus({
    required this.date,
    required this.statusFilled,
    required this.statusUnfilled,
    required this.statusVacation,
    required this.statusNotWorked,
  });
}
```
