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


When possible, avoid using fillForm because the field's default won't be used. Prefers to mutate data in the creation process.

        Forms\Components\Select::make('billing_address_id')
                        ->hidden(fn (Get $get) => $get('client_id') === null)
                        ->label('Adresse de facturation')
                        ->searchable()
                        ->preload()
                        ->relationship(
                            name: 'billingAddress',
                            modifyQueryUsing: fn (Builder $query, Get $get) => $query->where('client_id', $get('client_id'))
                        )
                        ->getOptionLabelFromRecordUsing(fn (Address $address) => $address->fullAddress)
                        ->getOptionLabelUsing(fn ($value): ?string => Address::withTrashed()->find($value)?->fullAddress)
                        ->createOptionForm([
                            Hidden::make('client_id')
                                ->required(),
                            ...AddressesRelationManager::getFormFieldsSchema(),
                        ])
                       -- ->createOptionAction(function (Action $action, Get $get) {
                       --     return $action
                       --         ->fillForm(fn (): array => [
                       --             'client_id' => $get('client_id'),
                       --         ])
                       --         ->modalHeading('Créer une adresse de facturation');
                       -- })
                       ++ ->createOptionModalHeading('Créer une adresse de facturation')
                       ++ ->createOptionUsing(fn (array $data, Get $get): int => Address::create([
                       ++     ...$data,
                       ++     Address::CLIENT_ID => $get('client_id'),
                       ++ ])->getKey())
                        ->required()
