To view the errors on a filament form when creating test, you can access the error bag on the instance from the Livewire component
        
        $page = Livewire::test(CreateSchedule::class)
            ->fillForm([
                Schedule::NAME => fake()->streetName().' '.fake()->asciify(),
                Schedule::START_DATE => Carbon::parse(fake()->dateTimeThisYear())->formatDate(),
                Schedule::END_DATE => Carbon::parse(fake()->dateTimeThisYear())->addYear()->formatDate(),
                Schedule::IS_MONDAY => true,
                Schedule::IS_TUESDAY => true,
                Schedule::IS_WEDNESDAY => true,
                Schedule::IS_THURSDAY => true,
                Schedule::IS_FRIDAY => true,
                Schedule::IS_SATURDAY => true,
                Schedule::IS_SUNDAY => true,
                'scheduleBlocks' => [
                    '1' => [

                        ScheduleBlock::START_TIME => '08:00',
                        ScheduleBlock::END_TIME => '17:00',
                    ],
                ],
            ])
            ->call('create');

        dd($page->instance()->getErrorBag());
